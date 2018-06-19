# gooddeeds
/*
 ============================================================================
 Name        : AssylYespanova.c
 Author      : Assyl Yespanova
 Description : "Good Deeds." Assignment 2. CSCI 151.
 ============================================================================
 */

#include <stdlib.h>
#include <stdio.h>
#include <string.h>

/* defining the struct for the deed list and its components */
typedef struct {
    int deed_id;
    char deed_name[55];
    int points_for_deed;
    int total_times_done;
    int total_points_earned;
} deed;

typedef struct {
	int deed_id;
	int number_of_times;
} daylog;

void print_output(deed deed[], int tot_num) /* function for printing out the table with necessary information at tasks 1, 2, 5 and 6 */
{
 printf ("NO   |           --------- deed name ----------                  |deed       |total       |total\n"
		 "     |                                                           |point      |times       |points\n");
 int a;
 for (a=tot_num-1; a>=0; a--)
 {
	 if (deed[a].total_times_done != 0)
	 {
printf("%i %i	%55s  %i	     %i	    	  %i\n", tot_num-a,
													 deed[a].deed_id, deed[a].deed_name,
													 deed[a].points_for_deed, deed[a].total_times_done,
													 deed[a].total_points_earned);
	 }
 }
}

void bubble_sort1 (deed x[]) /* function that sorts the total times from max to min (task 1) */
{
 	int i, j;
	for (i = 0; i < 18 - 1; i++)
	{

		for (j = 0; j < 18 - 1 - i; j++)
		{

			if ((x[j].total_times_done) > (x[j+1].total_times_done))
			{
				deed temp = x[j + 1];
				x[j + 1] = x[j];
				x[j] = temp;
			}
		}

	}
}

void bubble_sort2 (deed x[]) /* function that sorts the total points from max to min (task 2) */
{
 	int i, j;
	for (i = 0; i < 18 - 1; i++)
	{

		for (j = 0; j < 18 - 1 - i; j++)
		{	if ((x[j].total_points_earned) > (x[j+1].total_points_earned))

			{	deed temp = x[j + 1];
				x[j + 1] = x[j];
				x[j] = temp;
			}
		}

	}
}
void selection_sort_alphabet (deed x[]) /* function that sorts the deeds in alphabetical order (task 5) */
{
	int i, j, min;
	int tot_num = 18;

	for (i = 0; i < tot_num; i++) {

	min = i;
	for (j = i + 1; j < tot_num; j++) {
	if (strcmp(x[j].deed_name, x[min].deed_name) > 0) {
	min = j;
	}
	}


	deed temp = x[i];
	x[i] = x[min];
	x[min] = temp;
}
}

int binarySearch( deed x[], int size, char name [55])
{

	int low = 0;
	int high = size -1;

	while (low <= high) {

	int middle = (low + high) / 2;

	if (strcmp(x[middle].deed_name, name) == 0) {

	printf ("task6: search case1:\n%s %i times %i points earned\n", x[middle].deed_name, x[middle].total_times_done, x[middle].total_points_earned);
	return middle;
	}
	else if (strcmp(x[middle].deed_name, name) < 0)
	{
	high = middle + 1;
	}
	else
	{
	low = middle - 1;
	}

	}
	printf ("task6:search case2:\nNo such deed\n");
	return -1;
}

int main()
{

	setvbuf(stdout, NULL, _IONBF, 0);

	FILE *file1_deed_list;
	file1_deed_list = fopen("deed_list.txt", "r");

	FILE *file2_daylog;
	file2_daylog = fopen("daylog.txt", "r");

	int tot_num;

	fscanf(file1_deed_list, "%i", &tot_num);


	deed *deed_list;
	deed_list = malloc((tot_num)*sizeof(deed));

	int i;

	i = 0;

	do {

		fscanf(file1_deed_list,"%i %s %i", &deed_list[i].deed_id, deed_list[i].deed_name, &deed_list[i].points_for_deed);

		deed_list[i].total_points_earned = 0;
		deed_list[i].total_times_done = 0;
		i++;

		} while(!feof(file1_deed_list)) ;


	daylog *daylog1;
	daylog1 = malloc(28*sizeof(daylog));

	i = 0;
	do {
		fscanf(file2_daylog, "%i %i", &daylog1[i].deed_id, &daylog1[i].number_of_times);
		i++;
		} while(!feof(file2_daylog));

	for (i = 0; i < 28; i++)
	{
		deed_list[daylog1[i].deed_id - 1].total_times_done += daylog1[i].number_of_times;
		deed_list[daylog1[i].deed_id - 1].total_points_earned = deed_list[daylog1[i].deed_id - 1].total_times_done * deed_list[daylog1[i].deed_id - 1].points_for_deed;
	}


	printf ("      task1: after sort by deeds_total_times_done:\n\n");
	bubble_sort1(deed_list);
	print_output(deed_list, tot_num);
	printf ("\n");


	printf ("__________________________________________________________________________________________________\n\n");
	printf ("      task2: after sort by deeds_total_points_earned:\n\n");
	bubble_sort2(deed_list);
	print_output(deed_list, tot_num);
	printf ("\n");


	int sum1=0;
	for (i=0; i<18; i++)
	{
	sum1 += deed_list[i].total_times_done;
	}
	printf ("__________________________________________________________________________________________________\n\n");
	printf ("      task3: total deed_counts:%i\n", sum1);


	int sum2=0;
	for (i=0; i<tot_num; i++)
	{
	sum2 += deed_list[i].total_points_earned;
	}
	printf ("__________________________________________________________________________________________________\n\n");
	printf ("      task4: total deed_points:%i\n", sum2);


	printf ("__________________________________________________________________________________________________\n\n");
	printf ("      task5: after sort alphabetically:\n\n");
	selection_sort_alphabet(deed_list);
	print_output(deed_list, tot_num);
	printf ("\n");


	char name [55];
	printf ("__________________________________________________________________________________________________\n\n");

	while (1) {
	printf ("\nPlease, enter the name of the deed:\n");
	scanf ("%s", name);
	binarySearch (deed_list, tot_num, name);
	}


	fclose(file1_deed_list);
	fclose(file2_daylog);

return 0;


}

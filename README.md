# NTU sc1003-group-assignment
This is a sample of the SC1003 Group Assignment in Jupyter Notebook that I made in 2024 in Nanyang Technological University.
All files are included to run in a Windows environment.
- This readme can also be found in the .ipynb file provided.
> NOTE THAT ALL IMAGES are only inside the Jupyter Notebook file.

# **C.L.A.S.S.**
# **C**lass **L**ocator **A**ssistant **S**orter **S**ystem

This program uses the data fed in from a set of student records and automatically sorts and formats a human-readable output.

# Inputs
*   records.csv
*   weights.csv
*   tutors.csv

# Outputs
*   {tutorial_group}.csv
*   (Optional) Automated Email attachment


# Building *weights.csv*

To build weights.csv, the schools involved in the dataset were extracted in this script.

## 3 Considerations for ways to sort dataset by school:


1.   Sorting for Uniqueness:
  *        Sorting each group to ensure that there are no 2 members from the same school to allow for more unique skillsets within the team.
2.   Sorting by Alphabetical order:
  *        Sorting each group to ensure that there are not too many members from schools starting with the same letter. (NOT RECOMMENDED😞)
3.   Sorting by Competency: ✅
  *        Sorting each group by how each member is inclined in the project/assignment given on average from a given school
  *        Data needed for accuracy pertaining to competency towards a certain subject/topic/task

> Option 3 was chosen due to its practical use in the real world on top of being a better method for sorting compared to more arbitary vectors.

However, due to option 3 requiring external input, a survey was employed to gather results for a sample test case.

#Main Sorting Program
This is the main body of the sorting algorithm.
## Functions


*   getMean()
*   culDist() //function has been removed
*   tutSplit()
*   gpaSort()
*   cumuDist()
*   qlcSort()

###*getMean(arr)*:
Calculates the mean of a list *arr*.

returns *float*

###*getSD(arr, mean)*:
Calculates standard deviation from a list *arr* and the given mean *mean*.

returns *float*

###*cumuDist(arr)*:
Calculates cumulative distribution of values in a list *arr*.

returns *list* containing cumulation of values in ascending order.

###*tutSplit(arr, tutGrps)*:
builds a record of all tutorial groups from the dataset.

returns *list* containing all tutorial groups.

###*gpaSort(arr)*:
sorts the list *arr* by ascending order of CGPA.

returns *list* containing input list *arr* with CGPAs sorted by ascending order.

###*ceiling(flt)*:
returns the ceiling value of a number; AKA rounding the value **UP** to the nearest whole number.

returns *int* containing a rounded up value.

# **S.P.L.A.T.**
# **S**ort **P**rocedures **L**eading **A**ll **T**ypes

S.P.L.A.T. is defined in the program as qlcSort.

*qlcSort(arr,mean,gpnum,confidence)*:

qlcSort is a custom 3-layer sorting algorithm that sorts the dataset by:

1.   CGPA
2.   School
3.   Gender

These 3 factors have been prioritised accordingly by significance or impact to the group composition.

## Inputs:
*arr*: *list* containing all points of data to be sorted. This can be modified to include/exclude:


*   Students of a specific Tutorial Group
*   All students within the records.csv file

*mean*: mean CGPA of the overall dataset from records.csv used as a comparison in sorting.

*gpnum*: Alloted group size as specified by the user, sorts the output of each group according to the size.

*confidence*: pseudo-"confidence level" to imitate a system similar to Z-Score functions within a dataset - in this case CGPA records - that forms a standard normal distribution.

##CGPA

The CGPA sorting component utilises a variant of Quicksort to sort each point of data into the alloted bins. Bins are scaled with a multiplier based on the input *confidence* as a percentage(%) of the given mean.

Although not a perfect representation of a normal distibution, using a shifting mean can emulate a approximation of confidence level in a dataset that is broad enough to form into a standard normal distribution.

##Best-fit 'Attenuation'

> The CGPA algorithm determines the final group size for each group, using a best fit method for getting the group outputs.

This attenuation algorithm will always create teams with a lower member count than the group size, and will also provide marginally higher mean CGPA within such groups compared to groups with the orignal group size. This is meant to make up for the lower member count and thus increased workload across each member, and a higher CGPA mean may mean that members within such smaller groups are able to handle the corresponding workloads.

![CGPA_attenuation.png]

##School

The School sorting component utilises a custom sorting algorithm based on data collected.

This algorithm relies of 'weights' applied onto each school's member to determine their aptitude towards the assignment/subject/project. These factors determine the total weight of each group formed from the previous CGPA sorting algorithm, and the mean weight of the groups formed within the dataset.

The groups are sorted according to the weight mean, with higher weight groups and lower weight groups separated into bins. These bins are then selected to be swapped with the respective counterpart bins, e.g. low binned group to swap from high binned group.

>**NOTE:This sorting algorithm is meant to run ONLY ONCE, as within a group of 4-10 members, a single pass is adequate for maintaining a relatively balanced result across all groups without skewing the balanced mean CGPA per group.**

>In a standard scenario of a group size of 5, the skew of CGPA compared to the previous groupset would differ by 4%, where X is the sum of the CGPA of the original 5 members, M = 5, and the CGPA swap threshold is 20%.

    >˜f = [(X + 0.2N) / M] - (X/M)

From the same formula, applying the worst case value of M = 4 will yield a fluctuation of 5% and a best case value of M = 10 yielding a fluctuation of 2%.

![school_weight_binning.png]

##Example weighting: Coding Proficiency/Confidence
A survey was done by the team on peers and members of different schools within NTU, and the results are tabulated to provide the example weights inserted into *weights.csv*.

This survey gets surveyees to enter their school, and their confidence in coding/programming from the scale of 1 to 10. From the results, an mean(average) proficiency rating can be obtained. This is used to numerically compare individuals from one school to another.

###Problems Faced

*   Insufficient Responses
    * There was an uneven number of surveyees from each school, causing anomalous results from an individual heavily skewing the mean towards such results.
    >As outreach of the survey is limited, most of the dataset is comprised of individuals from CCDS at 29.1% (16 persons), with ADM, SPMS, and CoE equally having the least presence of 1.8% (1 person).
    * Limited responses across the whole dataset means that the survey's reliability is impacted, where results may be similar within each school but still do not reflect the real competency/confidence of individuals within the school.
      >**From the 18 schools retrieved from *records.csv*, the team managed to only obtain responses from 13 schools. The remaining schools without responses have placeholder proficiency values from speculation and prior understanding of the course types.**
* Inadequate Survey Runtime
    * The survey duration ran for about 2.5 weeks between 22nd October and 8th November. This may have posed an issue that individuals that have received the survey did not have enough time to submit their responses, causing a deficiency during data collection.

    >As of 8th November 2024 2230Hrs, only [55] responses were tabulated into the weightage in *weights.csv*, with a mean count of [4 (4.231)] from each school. Average anomalous count stands at ~[1] per school.

The survey results have been plotted out into a scatter chart to show averages of proficiency by school as shown:

[Survey Results](example.com)

![image.png]

As the above chart shows, EEE has the highest average proficiency at 5.8 (0.58) and CoE, SPMS, ADM, and WKW SCI the lowest at 1 (0.1).



#Gender

The Gender sorting component utilises a similar custom sorting algorithm based on the ratio balance of *Female*👩‍🎓 : *Male*👨‍🎓. As there are only 2 variables that all members **in the dataset** fall under, the algorithm only takes into account the count of *females* in each group.

Like the School algorithm, the groups are binned into 2 groups based on their gender dominance, with a threshold of 80% dominance of 1 gender triggering the binning process for the group.

As the priority for balance of gender within the groups is the lowest, the algorithm skips the swap if a best case pair is not present within the binned groups (dominance of either gender).

>**NOTE: This sorting algorithm runs ONLY in the best case, and ONLY ONCE so as to not affect the balance of the other 2 factors.**

>In this case, 'best case' refers to a matching opposing gender swap that has similar CGPA and proficiency.

![gender_sort.png]

#Output

After the 3 sorting algorithms have been run, the output is presented as a *dictionary* with a group number as a key and group members within as strings within a *list*.

# **S.P.A.M.**
##**S**mart **P**ersonalized **A**utomated **M**essaging

This is an automated messaging component of **C.L.A.S.S.**. It automatically takes the output files of **S.P.L.A.T.**, attaches the relevant documents for each tutor's tutorial group into Outlook and sends an email.

#Inputs:

*   {tutorial_group}.csv
*   tutors.csv

#Outputs:

*   Email Sent
*   Log Output



> **S.P.A.M.** utilises the default user account within Outlook on the running device to send the emails. Please check and change the *notDefault* flag to True along with *chosenUser* to match the device configuration.

A *log.txt* file will be outputted when the program finishes execution, containing the outcome of each message sent to the tutor, returning the result as a success or failure. If the result is a failure, it is due to an invalid tutorial group or email address within the *tutors.csv* file.

> **NOTE: This cannot be run inside hosted environments such as Google Colabatory, due to limitations of the *pywin32* library working only for Microsoft Outlook on Windows Systems.**



> **NOTE#2: Outlook(New) cannot be used as it no longer supports COM through the *pywin32* library, login to Outlook(Classic) to run this component.**

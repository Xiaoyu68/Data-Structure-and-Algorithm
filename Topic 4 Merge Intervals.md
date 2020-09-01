# Topic 4: Merge Intervals


[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

This pattern describes an efficient technique to deal with overlapping intervals. In a lot of problems involving intervals, we either need to find overlapping intervals or merge intervals if they overlap.
(6 patterns)
### Question 1 Merge Intervals

Given a list of intervals, merge all the overlapping intervals to produce a list that has only mutually exclusive intervals.

Example 1:
```sh
Intervals: [[1,4], [2,5], [7,9]]
Output: [[1,5], [7,9]]
Explanation: Since the first two intervals [1,4] and [2,5] overlap, we merged them into 
one [1,5].
```
Example 2:
```sh
Intervals: [[6,7], [2,4], [5,9]]
Output: [[2,4], [5,9]]
Explanation: Since the intervals [6,7] and [5,9] overlap, we merged them into one [5,9].
```
Example 3:
```sh
Intervals: [[1,4], [2,6], [3,5]]
Output: [[1,6]]
Explanation: Since all the given intervals overlap, we merged them into one.
```
The diagram above clearly shows a merging approach. Our algorithm will look like this:

 - Sort the intervals on the start time to ensure a.start <= b.start
 - If ‘a’ overlaps ‘b’ (i.e. b.start <= a.end), we need to merge them into a new interval ‘c’ such that:
     - c.start = a.start
     - c.end = max(a.end, b.end)
We will keep repeating the above two steps to merge ‘c’ with the next interval if it overlaps with ‘c’.


### Solution:
 - C++ Solution:
```sh
Class Interval{
public:
    int start = 0;
    int end = 0;
    Interval(int start, int end){
        this->start = start;
        this->end = end;
    }
};
Class solution{
public:
    static  vector<Interval> merge(vector<Interval>& intervals){
        vector<Interval> res;
        if(intervals.size() < 2){
            return intervals;
        }
        sort(intervals.begin(), intervals.end(),[](Interval& a, Interval& b){
           return a.start<b.start; 
        });
        res.push_back(intervals[0]);
        for(int i  = 1;i < intervals.size();i++){
            if(res.back().end >= intervals[i].start){
                res.back().end = max(res.back().end, intervals[i].end);
            }
            else{
                res.push_back(intervals[i]);
            }
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
 Class Interval{
     int start;
     int end;
     
     public Interval(int start, int end){
         this.start = start;
         this.end = end;
     }
 }
Class solution{
    public static List<Interval> merge(List<Interval> intervals){
        List<Interval> res = new ArrayList<Interval>();
        if(intervals.size() < 2){
            return intervals;
        }
        Collections.sort(intervals,(a,b)->Integer.compare(a.start,b.start));
        res.add(intervals[0]);
        for(int i = 1;i < intervals.size();i++){
            if(intervals.get(i).start <= res.getLast().end){
                res.getLast().end = Math.max(res.getLast().end, intervals.get(i).end);
            }
            else{
                res.add(intervals.get(i));
            }
        }
        return res;
    }
};
```
Time complexity #
The time complexity of the above algorithm is O(N * logN)O(N∗logN), where ‘N’ is the total number of intervals. We are iterating the intervals only once which will take O(N)O(N), in the beginning though, since we need to sort the intervals, our algorithm will take O(N * logN)O(N∗logN).

Space complexity #
The space complexity of the above algorithm will be O(N)O(N) as we need to return a list containing all the merged intervals. We will also need O(N)O(N) space for sorting. For Java, depending on its version, Collection.sort() either uses Merge sort or Timsort, and both these algorithms need O(N)O(N) space. Overall, our algorithm has a space complexity of O(N)O(N).

### Question 2 Insert Interval
Given a list of non-overlapping intervals sorted by their start time, insert a given interval at the correct position and merge all necessary intervals to produce a list that has only mutually exclusive intervals.
 - Example 1:
```sh
Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,6]
Output: [[1,3], [4,7], [8,12]]
Explanation: After insertion, since [4,6] overlaps with [5,7], we merged them into one [4,7].
```
 - Example 2:
```sh
Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,10]
Output: [[1,3], [4,12]]
Explanation: After insertion, since [4,10] overlaps with [5,7] & [8,12], we merged them into [4,12].
```
 - Example 3:
```sh
Input: Intervals=[[2,3],[5,7]], New Interval=[1,4]
Output: [[1,4], [5,7]]
Explanation: After insertion, since [1,4] overlaps with [2,3], we merged them into one [1,4].
```
If the given list was not sorted, we could have simply appended the new interval to it and used the merge() function from Merge Intervals. But since the given list is sorted, we should try to come up with a solution better than O(N * logN)O(N∗logN)

When inserting a new interval in a sorted list, we need to first find the correct index where the new interval can be placed. In other words, we need to skip all the intervals which end before the start of the new interval. So we can iterate through the given sorted listed of intervals and skip all the intervals with the following condition:

intervals[i].end < newInterval.start
Once we have found the correct place, we can follow an approach similar to Merge Intervals to insert and/or merge the new interval. Let’s call the new interval ‘a’ and the first interval with the above condition ‘b’. There are five possibilities:

The diagram above clearly shows the merging approach. To handle all four merging scenarios, we need to do something like this:

    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
Our overall algorithm will look like this:

Skip all intervals which end before the start of the new interval, i.e., skip all intervals with the following condition:
    intervals[i].end < newInterval.start
Let’s call the last interval ‘b’ that does not satisfy the above condition. If ‘b’ overlaps with the new interval (a) (i.e. b.start <= a.end), we need to merge them into a new interval ‘c’:
    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
We will repeat the above two steps to merge ‘c’ with the next overlapping interval.


### Solution:
 - C++ Solution:
```sh
Class Interval{
    int start;
    int end;
    public Interval(int start, int end){
        this->start = start;
        this->end = end;
    }
}
Class solution{
public:
    static vector<Interval> insert(const vector<Interval>& intervals, Interval newInterval){
        vector<Interval> res;
        int i = 0;
        while(i < intervals.size() && intervals[i].end < newInterval.start){
            res.push_back(intervals[i]);
            i++;
        }
        while(i < intervals.size() && intervals[i].start <= newInterval.end){
            newInterval.start = min(intervals[i].start,newInterval.start);
            newInterval.end = max(intervals[i].end, newInterval.end);
        }
        res.push_back(newInterval);
        while(i < intervals.size()){
            res.push_back(intervals[i]);
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
 Class Interval{
     int start;
     int end;
     public Interval(int start, int end){
         this.start = start;
         this.end = end;
     }
 }
Class solution{
    public static List<Interval> insert(const List<Interval> intervals, Interval newInterval){
        List<Interval> res = new LinkedList<>();
        int i = 0;
        while(i < intervals.size() && intervals.get(i).end < newInterval.start){
            res.add(intervals.get(i));
            i++;
        }
        while(i < intervals.size() && intervals.get(i).start <= newInterval.end){
            newInterval.start = Math.min(intervals.get(i).start,newInterval.start);
            newInterval.end = Math.max(intervals.get(i).end, newInterval.end);
        }
        res.push_back(newInterval);
        while(i < intervals.size()){
            res.add(intervals.get(i));
        }
        return res;
    }
};
```
Time Complexity: O(N)
Space Complexity: O(1)

### Question 3 Intervals Intersection
Given two lists of intervals, find the intersection of these two lists. Each list consists of disjoint intervals sorted on their start time.
 - Example 1:
```sh
Input: arr1=[[1, 3], [5, 6], [7, 9]], arr2=[[2, 3], [5, 7]]
Output: [2, 3], [5, 6], [7, 7]
Explanation: The output list contains the common intervals between the two lists.
```
 - Example 2:
```sh
Input: arr1=[[1, 3], [5, 7], [9, 12]], arr2=[[5, 10]]
Output: [5, 7], [9, 10]
Explanation: The output list contains the common intervals between the two lists.
```
This problem follows the Merge Intervals pattern. As we have discussed under Insert Interval, there are five overlapping possibilities between two intervals ‘a’ and ‘b’. A close observation will tell us that whenever the two intervals overlap, one of the interval’s start time lies within the other interval. This rule can help us identify if any two intervals overlap or not.

Now, if we have found that the two intervals overlap, how can we find the overlapped part?

Again from the above diagram, the overlapping interval will be equal to:

    start = max(a.start, b.start)
    end = min(a.end, b.end) 
That is, the highest start time and the lowest end time will be the overlapping interval.

So our algorithm will be to iterate through both the lists together to see if any two intervals overlap. If two intervals overlap, we will insert the overlapped part into a result list and move on to the next interval which is finishing early. 

### Solution:
 - C++ Solution:
```sh
Class Interval{
public:
    int start = 0;
    int end = 0;
    
    Interval(int start, int end){
        this->start = start;
        this->end = end;
    }
};
Class solution{
public:
    static vector<Interval> merge(const vector<Interval>& arr1, const vecter<Interval>& arr2){
        int i = 0;
        int j = 0;
        vector<Interval> res;
        while(i < arr1.size() && j < arr2.size()){
            if(arr1[i].start >= arr2[j].start && arr1[i].start <= arr2[j].end || arr2[j].start >= arr1[i].start && arr2[j].start <= arr1[i].end){
                int nstart = max(arr1[i].start,arr2[j].start);
                int nend = min(arr1[i].end,arr2[j].end);
                res.push_back(new Interval(nstart,nend);
            }
            if(arr1[i].end < arr2[j].end){
                i++;
            }
            else{
                j++;
            }
        }
        return res;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
 Class Interval{
    int start = 0;
    int end = 0;
    
    public Interval(int start, int end){
        this.start = start;
        this.end = end;
    }
};
Class solution{
     static List<Interval> merge(List<Interval> arr1, List<Interval> arr2){
        int i = 0;
        int j = 0;
        List<Interval> res = new ArrayList<>();
        while(i < arr1.size() && j < arr2.size()){
            if(arr1.get(i).start >= arr2.get(j).start && arr1.get(i).start <= arr2.get(j).end || arr2.get(j).start >= arr1.get(i).start && arr2.get(j).start <= arr1.get(i).end){
                int nstart = Math.max(arr1.get(i).start,arr2.get(j).start);
                int nend = Math.min(arr1.get(i).end,arr2.get(j).end);
                res.add(new Interval(nstart,nend);
            }
            if(arr1.get(i).end < arr2.get(j).end){
                i++;
            }
            else{
                j++;
            }
        }
        return res;
    }
};
```
Time Complexity: O(N + M)
Space Complexity: O(1)

### Question 4 Conflicting Appointments
Given an array of intervals representing ‘N’ appointments, find out if a person can attend all the appointments.
 - Example 1:
```sh
Appointments: [[1,4], [2,5], [7,9]]
Output: false
Explanation: Since [1,4] and [2,5] overlap, a person cannot attend both of these appointments.
```
 - Example 2:
```sh
Appointments: [[6,7], [2,4], [8,12]]
Output: true
Explanation: None of the appointments overlap, therefore a person can attend all of them.
```
 - Example 3:
```sh
Appointments: [[4,5], [2,3], [3,6]]
Output: false
Explanation: Since [4,5] and [3,6] overlap, a person cannot attend both of these appointments.
```
Time Complexity:
Q(N+M)
Space Complexity:
O(1)
### Solution:
 - C++ Solution:
```sh
Class Interval{
public:
    int start;
    int end;
    
    Interval(int start, int end){
        this->start;
        this->end;
    }
};
Class solution{
public:
    static bool canAttendAllAppointments(vector<Interval>& intervals){
        sort(intervals.begin(),intervals.end());
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i].start < inervals[i-1].end){
                return false;
            }
        }
        return true;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
 Class Intervals{
     int start;
     int end;
    public Intervals(int start, int end){
        this.start = start;
        this.end = end;
    }
 }
Class solution{
    public static boolean canAttendAllApontments(Interval[] intervals){
        Arrays.sort(intervals,(a,b)->Integer.compare(a.start,b.start));
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i-1].end > intervals[i].start){
                return false;
            }
        return true;
    }
};
```
Time Complexity: O(N * log(N))
Space Complexity: O(N)

### Question 5 Minimun Meeting Rooms
Given a list of intervals representing the start and end time of ‘N’ meetings, find the minimum number of rooms required to hold all the meetings.
 - Example 1:
```sh
Meetings: [[1,4], [2,5], [7,9]]
Output: 2
Explanation: Since [1,4] and [2,5] overlap, we need two rooms to hold these two meetings. [7,9] can 
occur in any of the two rooms later.
```
 - Example 2:
```sh
Meetings: [[6,7], [2,4], [8,12]]
Output: 1
Explanation: None of the meetings overlap, therefore we only need one room to hold all meetings.
```
 - Example 3:
```sh
Meetings: [[1,4], [2,3], [3,6]]
Output:2
Explanation: Since [1,4] overlaps with the other two meetings [2,3] and [3,6], we need two rooms to 
hold all the meetings.
```
 - Example 4:
```sh
Meetings: [[4,5], [2,3], [2,4], [3,5]]
Output: 2
Explanation: We will need one room for [2,3] and [3,5], and another room for [2,4] and [4,5].
 
Here is a visual representation of Example 4:
```
Let’s take the above-mentioned example (4) and try to follow our Merge Intervals approach:

Meetings: [[4,5], [2,3], [2,4], [3,5]]

Step 1: Sorting these meetings on their start time will give us: [[2,3], [2,4], [3,5], [4,5]]

Step 2: Merging overlapping meetings:

[2,3] overlaps with [2,4], so after merging we’ll have => [[2,4], [3,5], [4,5]]
[2,4] overlaps with [3,5], so after merging we’ll have => [[2,5], [4,5]]
[2,5] overlaps [4,5], so after merging we’ll have => [2,5]
Since all the given meetings have merged into one big meeting ([2,5]), does this mean that they all are overlapping and we need a minimum of four rooms to hold these meetings? You might have already guessed that the answer is NO! As we can clearly see, some meetings are mutually exclusive. For example, [2,3] and [3,5] do not overlap and can happen in one room. So, to correctly solve our problem, we need to keep track of the mutual exclusiveness of the overlapping meetings.

Here is what our strategy will look like:

We will sort the meetings based on start time.
We will schedule the first meeting (let’s call it m1) in one room (let’s call it r1).
If the next meeting m2 is not overlapping with m1, we can safely schedule it in the same room r1.
If the next meeting m3 is overlapping with m2 we can’t use r1, so we will schedule it in another room (let’s call it r2).
Now if the next meeting m4 is overlapping with m3, we need to see if the room r1 has become free. For this, we need to keep track of the end time of the meeting happening in it. If the end time of m2 is before the start time of m4, we can use that room r1, otherwise, we need to schedule m4 in another room r3.
We can conclude that we need to keep track of the ending time of all the meetings currently happening so that when we try to schedule a new meeting, we can see what meetings have already ended. We need to put this information in a data structure that can easily give us the smallest ending time. A Min Heap would fit our requirements best.

So our algorithm will look like this:

Sort all the meetings on their start time.
Create a min-heap to store all the active meetings. This min-heap will also be used to find the active meeting with the smallest end time.
Iterate through all the meetings one by one to add them in the min-heap. Let’s say we are trying to schedule the meeting m1.
Since the min-heap contains all the active meetings, so before scheduling m1 we can remove all meetings from the heap that have ended before m1, i.e., remove all meetings from the heap that have an end time smaller than or equal to the start time of m1.
Now add m1 to the heap.
The heap will always have all the overlapping meetings, so we will need rooms for all of them. Keep a counter to remember the maximum size of the heap at any time which will be the minimum number of rooms needed.

### Solution:
 - C++ Solution:
```sh
Class Meeting{
    public:
        int start = 0;
        int end = 0;
    Meeting(int start, int end){
        this->start = start;
        this->end = end;
    }
}
Class solution{
public:
    struct cmp{
        bool operator()(const Meeting &x, const Meeting &y){
            return x.end>y.end;
        }
    }
    static int findMinimumMeetingRooms(vector<Meeting>& meetings){
        if(meetings.empty()){
            return 0;
        }
        sort(meetings.begin(),meetings.end(),[](Meeting &a, Meeting &b){
            return a.start < b.start;
        });
        priority_queue<Meeting, vector<Meeting>, cmp> pq;
        int minMeeting = 0;
        for(auto meeting :meetings){
            while(!pq.empty() && pq.top().end <= meeting.start){
                pq.pop();
            }
            pq.push(meeting);
            minMeeting = max(minMeeting, pq.size());
        }
        return minMeeting;
    }
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;

```
Time Complexity: O(N*log(N))
Space Complexity: O(N)

### Question 6 Maximun CPU Load
We are given a list of Jobs. Each job has a Start time, an End time, and a CPU load when it is running. Our goal is to find the maximum CPU load at any time if all the jobs are running on the same machine.
 - Example 1:
```sh
Jobs: [[1,4,3], [2,5,4], [7,9,6]]
Output: 7
Explanation: Since [1,4,3] and [2,5,4] overlap, their maximum CPU load (3+4=7) will be when both the 
jobs are running at the same time i.e., during the time interval (2,4).
```
 - Example 2:
```sh
Jobs: [[6,7,10], [2,4,11], [8,12,15]]
Output: 15
Explanation: None of the jobs overlap, therefore we will take the maximum load of any job which is 15.
```
 - Example 3:
```sh
Jobs: [[1,4,2], [2,4,1], [3,6,5]]
Output: 8
Explanation: Maximum CPU load will be 8 as all jobs overlap during the time interval [3,4]. 
```
The problem follows the Merge Intervals pattern and can easily be converted to Minimum Meeting Rooms. Similar to ‘Minimum Meeting Rooms’ where we were trying to find the maximum number of meetings happening at any time, for ‘Maximum CPU Load’ we need to find the maximum number of jobs running at any time. We will need to keep a running count of the maximum CPU load at any time to find the overall maximum load.

### Solution:
 - C++ Solution:
```sh
Class Job{
    int start;
    int end;
    int cpuLoad;
public:
    Job(int start, int end, int cpuLoad){
        this->start = start;
        this->end = end;
        this->cpuLoad = cpuLoad;
    }
};
Class solution{
public:
    struct cmp{
        bool operator()(Job a, Job b){
            return a.end > b.end;
        }
    }
    static int findMaxCPULoad(vector<Job>& jobs){
        if(jobs.empty()){
            return 0;
        }
        sort(jobs.begin(),jobs.end(),[](Job &a, Job &b){
            return a.start < b.start;
        });
        priority_queue<Job, vector<Job>, cmp> pq;
        int minC = 0;
        int curr = 0;
        for(auto job :jobs){
            while(!pq.empty() && pq.top().end <= job.start){
                curr -= pop.top().cpuLoad;
                pop.pop();
            }
            pq.push(job);
            curr += job.cpuLoad;
            minC = max(minC, curr);
        }
        return minC;
};
```
  - Java Solution:
 ```sh
 import java.util.Arrays;
class Job {
  int start;
  int end;
  int cpuLoad;

  public Job(int start, int end, int cpuLoad) {
    this.start = start;
    this.end = end;
    this.cpuLoad = cpuLoad;
  }
};

class MaximumCPULoad {

  public static int findMaxCPULoad(List<Job> jobs) {
    // sort the jobs by start time
    Collections.sort(jobs, (a, b) -> Integer.compare(a.start, b.start));

    int maxCPULoad = 0;
    int currentCPULoad = 0;
    PriorityQueue<Job> minHeap = new PriorityQueue<>(jobs.size(), (a, b) -> Integer.compare(a.end, b.end));
    for (Job job : jobs) {
      // remove all jobs that have ended
      while (!minHeap.isEmpty() && job.start > minHeap.peek().end)
        currentCPULoad -= minHeap.poll().cpuLoad;

      // add the current job into the minHeap
      minHeap.offer(job);
      currentCPULoad += job.cpuLoad;
      maxCPULoad = Math.max(maxCPULoad, currentCPULoad);
    }
    return maxCPULoad;
  }
```
Time Complexity: O(N^2)
Space Complexity: O(N)

### Question 7 Employee Free Time
For ‘K’ employees, we are given a list of intervals representing the working hours of each employee. Our goal is to find out if there is a free interval that is common to all employees. You can assume that each list of employee working hours is sorted on the start time.
 - Example 1:
```sh
Input: Employee Working Hours=[[[1,3], [5,6]], [[2,3], [6,8]]]
Output: [3,5]
Explanation: Both the employess are free between [3,5].
```
 - Example 2:
```sh
Input: Employee Working Hours=[[[1,3], [9,12]], [[2,4]], [[6,8]]]
Output: [4,6], [8,9]
Explanation: All employess are free between [4,6] and [8,9].
```
 - Example 3:
```sh
Input: Employee Working Hours=[[[1,3]], [[2,4]], [[3,5], [7,9]]]
Output: [5,7]
Explanation: All employess are free between [5,7].
```
This problem follows the Merge Intervals pattern. Let’s take the above-mentioned example (2) and visually draw it:

Input: Employee Working Hours=[[[1,3], [9,12]], [[2,4]], [[6,8]]]
Output: [4,6], [8,9]
svg viewer
One simple solution can be to put the working hours of all employees in a list and sort them on the start time. Then we can iterate through the list to find the gaps. Let’s dig deeper. Sorting the intervals of the above example will give us:

[1,3], [2,4], [6,8], [9,12]
We can now iterate through these intervals, and whenever we find non-overlapping intervals (e.g., [2,4] and [6,8]), we can calculate a free interval (e.g., [4,6]). This algorithm will take O(N * logN)O(N∗logN) time, where ‘N’ is the total number of intervals. This time is needed because we need to sort all the intervals. The space complexity will be O(N)O(N), which is needed for sorting. Can we find a better solution?

Using a Heap to Sort the Intervals #
One fact that we are not utilizing is that each employee list is individually sorted!

How about we take the first interval of each employee and insert it in a Min Heap. This Min Heap can always give us the interval with the smallest start time. Once we have the smallest start-time interval, we can then compare it with the next smallest start-time interval (again from the Heap) to find the gap. This interval comparison is similar to what we suggested in the previous approach.

Whenever we take an interval out of the Min Heap, we can insert the next interval of the same employee. This also means that we need to know which interval belongs to which employee.

### Solution:
 - C++ Solution:
```sh
class Interval {
 public:
  int start = 0;
  int end = 0;

  Interval(int start, int end) {
    this->start = start;
    this->end = end;
  }
};

class EmployeeFreeTime {
 public:
  struct startCompare {
    bool operator()(const pair<Interval, pair<int, int>> &x,
                    const pair<Interval, pair<int, int>> &y) {
      return x.first.start > y.first.start;
    }
  };

  static vector<Interval> findEmployeeFreeTime(const vector<vector<Interval>> &schedule) {
    vector<Interval> result;
    if (schedule.empty()) {
      return result;
    }

    // PriorityQueue to store one interval from each employee
    priority_queue<pair<Interval, pair<int, int>>, vector<pair<Interval, pair<int, int>>>,
                   startCompare>
        minHeap;

    // insert the first interval of each employee to the queue
    for (int i = 0; i < schedule.size(); i++) {
      minHeap.push(make_pair(schedule[i][0], make_pair(i, 0)));
    }

    Interval previousInterval = minHeap.top().first;
    while (!minHeap.empty()) {
      auto queueTop = minHeap.top();
      minHeap.pop();
      // if previousInterval is not overlapping with the next interval, insert a free interval
      if (previousInterval.end < queueTop.first.start) {
        result.push_back({previousInterval.end, queueTop.first.start});
        previousInterval = queueTop.first;
      } else {  // overlapping intervals, update the previousInterval if needed
        if (previousInterval.end < queueTop.first.end) {
          previousInterval = queueTop.first;
        }
      }

      // if there are more intervals available for the same employee, add their next interval
      vector<Interval> employeeSchedule = schedule[queueTop.second.first];
      if (employeeSchedule.size() > queueTop.second.second + 1) {
        minHeap.push(make_pair(employeeSchedule[queueTop.second.second + 1],
                               make_pair(queueTop.second.first, queueTop.second.second + 1)));
      }
    }

    return result;
  }
};
```
Time Complexity: O(N*log(N))
Space Complexity: O(k)
### Todos

 - Write MORE Tests
 - Add Night Mode

License
----

MIT

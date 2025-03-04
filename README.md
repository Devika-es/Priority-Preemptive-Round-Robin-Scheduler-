# Priority-Preemptive-Round-Robin-Scheduler-


#include <stdio.h>
struct Process {
    int process_id;
    int arrival_time;
    int burst_time;
    int priority;
};

int main() {
    int num_processes, time_quantum;

    printf("Enter the number of processes: ");
    scanf("%d", &num_processes);

    printf("Enter the time quantum for Round Robin scheduling: ");
    scanf("%d", &time_quantum);

    struct Process processes[num_processes];
    int remaining_burst_time[num_processes];
    int completion_time[num_processes];
    int current_time = 0;

    for (int i = 0; i < num_processes; i++) {
        printf("\nEnter details for Process %d:\n", i + 1);
        processes[i].process_id = i + 1;

        printf("Arrival Time: ");
        scanf("%d", &processes[i].arrival_time);

        printf("Burst Time: ");
        scanf("%d", &processes[i].burst_time);

        printf("Priority (Higher number indicates higher priority): ");
        scanf("%d", &processes[i].priority);

        remaining_burst_time[i] = processes[i].burst_time;
    }
    int completed = 0;
    while (completed < num_processes) {
        int chosen_process = -1;
        int highest_priority = -1;
        for (int i = 0; i < num_processes; i++) {
            if (processes[i].arrival_time <= current_time && remaining_burst_time[i] > 0) {
                if (processes[i].priority > highest_priority) {
                    highest_priority = processes[i].priority;
                    chosen_process = i;
                }
            }
        }
        if (chosen_process == -1) {
            current_time++;
        } else {
            if (remaining_burst_time[chosen_process] > time_quantum) {
                current_time += time_quantum;
                remaining_burst_time[chosen_process] -= time_quantum;
            } else {
                current_time += remaining_burst_time[chosen_process];
                completion_time[chosen_process] = current_time;
                remaining_burst_time[chosen_process] = 0;
                completed++;
            }
        }
    }
    printf("\n");
    printf("Process\t Arrival Time\t Burst Time\tPriority    Completion Time\tTurnaround Time\t  Waiting Time\n");
    int total_waiting_time = 0;
    int total_turnaround_time = 0;
    for (int i = 0; i < num_processes; i++) {
        int turnaround_time = completion_time[i] - processes[i].arrival_time;
        int waiting_time = turnaround_time - processes[i].burst_time;
        printf("%d\t %d\t\t %d\t\t%d\t    %d\t\t\t%d\t\t  %d\n", processes[i].process_id, processes[i].arrival_time, processes[i].burst_time, processes[i].priority, completion_time[i], turnaround_time, waiting_time);
        total_waiting_time += waiting_time;
        total_turnaround_time += turnaround_time;
    }
    printf("\nAverage Waiting Time: %.2f\n", (float)total_waiting_time / num_processes);
    printf("Average Turnaround Time: %.2f\n", (float)total_turnaround_time / num_processes);
    return 0;
}

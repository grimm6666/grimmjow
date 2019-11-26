# grimmjow


round robin algorithm in java code

import java.util.List;

public class RoundRobin {
    
    int [] temp;
    int [] tempWaitinTime;
    int commBT, k, tq;
    int[][] d;
    int btcache;
    
    void start( ){
        for (int i = 0; i < d.length; i++) {
            int bt  = d[i][1];
            if( bt > 0){
                if( bt <= tq){
                    temp[i] = btcache+bt;
                    btcache = temp[i];
                    k += bt;
                    bt -= bt;
                    
                }
                else{
                    temp[i] = btcache+tq;
                    btcache = temp[i];
                    bt -= tq;
                    k += tq;
                }
                
                d[i][1] = bt;
                
            }
        }
        if( k!= commBT)
        start();
    }
    
    private void display(List<Job> jobList) {
        float avgTurnArroundTime = 0;
        float avgWaitigTime = 0;
        int c = 1;
       
      System.out.println("Process ID | Turnaround time | Waiting time ");
        
        Object[] job = jobList.toArray();
        for (int i : temp) {
            Job job1 = (Job) job[c-1];
            System.out.println( " " + c + "  " + i +"   "+(i-job1.getCpuTime()));
           
            avgTurnArroundTime += i;
            avgWaitigTime += (i-job1.getCpuTime());
            c++;
        }
        System.out.println("===============================================");
        System.out.println( "Avg waiting time = " + avgWaitigTime/temp.length);
        System.out.println("===============================================");
        System.out.println( "Avg turn round time = " + avgTurnArroundTime/temp.length);
        System.out.println("===============================================");
    }
    public void run(List<Job> jobList, int quantum) {
        
        int pcount = jobList.size();
        d = new int[pcount][2];
        
        temp = new int[pcount];
        int count = 0;
        for(Job job:jobList){
            d[count][0] = count;
            
            int m = job.getCpuTime();
            d[count][1] = m;
            
            commBT += m;
            count++;
        }
        
        tq = quantum;
        start();
        display(jobList);
        
    }
}


AllocationStrategy.java


import java.util.ArrayList;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;
import java.util.Queue;



public abstract class AllocationStrategy {
    protected List<Job> Jobs;
    protected ArrayList<Job> Queue;
    
    public AllocationStrategy(List<Job> jobs) {
        super();
        Jobs = jobs;
    }
    
    public abstract void run();
   
}

Job.java




public class Job {
    private int id, submitTime, CPUTime, CPUTimeLeft;
    
    private int startTime = 0, endTime = 0;
    
    
    public int ProcessCompletionTime;
    public int processArrivalTime;
    public int waitingTime;
    public int turnAroundTime;
    private JobFinishEvent evt;
    
    private int arrivalTime,cpuTime,processId;
    
    public Job(int id, int submitTime, int CPUTime, JobFinishEvent evt) {
        super();
        this.id = id;
        this.submitTime = submitTime;
        this.CPUTime = CPUTime;
        this.CPUTimeLeft = CPUTime;
        this.evt = evt;
    }
    
    public Job(int processId, int arrivalTime, int cpuTime) {
        
        this.processId = processId;
        this.arrivalTime = arrivalTime;
        this.cpuTime = cpuTime;
        
    }
    
    public void start(int sysTime) {
        startTime = sysTime;
    }
    
    public void tick(int sysTime) {
        CPUTimeLeft --;
        if (CPUTimeLeft <= 0){
            endTime = sysTime;
            evt.onFinish(this);
        }
        
    }
    
    public int getId() {
        return id;
    }
    
    public void setId(int id) {
        this.id = id;
    }
    
    public int getSubmitTime() {
        return submitTime;
    }
    
    public void setSubmitTime(int submitTime) {
        this.submitTime = submitTime;
    }
    
    public int getCPUTime() {
        return CPUTime;
    }
    
    public void setCPUTime(int cPUTime) {
        CPUTime = cPUTime;
    }
    
    public int getCPUTimeLeft() {
        return CPUTimeLeft;
    }
    
    public void setCPUTimeLeft(int cPUTimeLeft) {
        CPUTimeLeft = cPUTimeLeft;
    }
    
    public int getStartTime() {
        return startTime;
    }
    
    public void setStartTime(int startTime) {
        this.startTime = startTime;
    }
    
    public int getEndTime() {
        return endTime;
    }
    
    public void setEndTime(int endTime) {
        this.endTime = endTime;
    }
    
    public int getArrivalTime() {
        return arrivalTime;
    }
    
    public void setArrivalTime(int arrivalTime) {
        this.arrivalTime = arrivalTime;
    }
    
    public int getCpuTime() {
        return cpuTime;
    }
    
    public void setCpuTime(int cpuTime) {
        this.cpuTime = cpuTime;
    }
    
    public int getProcessId() {
        return processId;
    }
    
    public void setProcessId(int processId) {
        this.processId = processId;
    }
    
    
}




JobFinishEvent.java



public interface JobFinishEvent {
    public void onFinish(Job j);
}

RR scheduling algorithm java codes

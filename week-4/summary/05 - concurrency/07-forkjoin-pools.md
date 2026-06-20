# ForkJoin Pools

ForkJoinPool is a special type of thread pool designed for divide-and-conquer problems where you can break a big task into smaller subtasks, solve them in parallel, and combine the results. It is different from a regular thread pool because it uses a work-stealing algorithm — when a thread finishes its own tasks it can steal tasks from other threads' queues which helps balance the load and keeps all threads busy. This makes it very efficient for recursive parallel problems. The main class is ForkJoinTask which has two implementations — RecursiveTask that returns a result and RecursiveAction that does not return anything. You override the compute() method which checks if the task is small enough to solve directly, if not it splits into subtasks, forks them to run in parallel, and then joins the results. The most common use is parallel divide-and-conquer algorithms like parallel sort, parallel merge, or processing large datasets by splitting them into chunks. Java also uses ForkJoinPool internally for parallel streams — when you call parallelStream() the work is distributed across a ForkJoinPool. The default pool size is the number of CPU cores which makes sense for CPU-bound tasks. You can create your own ForkJoinPool but in most cases the common pool that comes with Java is enough.

```java
import java.util.concurrent.*;

class SumTask extends RecursiveTask<Long> {
    private static final int THRESHOLD = 10000;
    private int[] array;
    private int start, end;

    SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        // Small enough — solve directly
        if (end - start <= THRESHOLD) {
            long sum = 0;
            for (int i = start; i < end; i++) sum += array[i];
            return sum;
        }

        // Split into subtasks
        int mid = (start + end) / 2;
        SumTask left = new SumTask(array, start, mid);
        SumTask right = new SumTask(array, mid, end);

        left.fork();   // run left in parallel
        long rightResult = right.compute();   // compute right in current thread
        long leftResult = left.join();        // wait for left to finish

        return leftResult + rightResult;
    }
}

public class ForkJoinDemo {
    public static void main(String[] args) {
        int[] array = new int[100000];
        for (int i = 0; i < array.length; i++) array[i] = i + 1;

        ForkJoinPool pool = new ForkJoinPool();
        long sum = pool.invoke(new SumTask(array, 0, array.length));

        System.out.println("Sum: " + sum);   // 5000050000
    }
}
```

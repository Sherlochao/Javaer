## wait notify

```
public class WaitNotifyTest {

    private static final Object object = new Object();

    public static void main(String[] args) throws InterruptedException {
        Thread threadA = new Thread(() -> {
            while (true) {
                synchronized (object) {
                    System.out.println("A");
                    object.notify();
                    try {
                        object.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        Thread threadB = new Thread(() -> {
            while (true) {
                synchronized (object) {
                    System.out.println("B");
                    object.notify();
                    try {
                        object.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        threadA.start();
        threadB.start();
    }
}

```

## ReentrantLock

```
public class ReentrantLockTest {

    private static ReentrantLock lock = new ReentrantLock();
    private static Condition condition1 = lock.newCondition();
    private static Condition condition2 = lock.newCondition();

    public static void main(String[] args) {
        Thread threadA = new Thread(() -> {
            while (true) {
                lock.lock();
                System.out.println("A");
                try {
                    condition2.signal();
                    condition1.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        });

        Thread threadB = new Thread(() -> {
            while (true) {
                lock.lock();
                System.out.println("B");
                try {
                    condition1.signal();
                    condition2.await();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    lock.unlock();
                }
            }
        });

        threadA.start();
        threadB.start();
    }
}

```

## volatile

```
public class VolatileTest {

    private static volatile boolean flag = false;

    public static void main(String[] args) {
        Thread threadA = new Thread(() -> {
            while (true) {
                if (!flag) {
                    System.out.println("A");
                    flag = true;
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        Thread threadB = new Thread(() -> {
            while (true) {
                if (flag) {
                    System.out.println("B");
                    flag = false;
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        });

        threadA.start();
        threadB.start();
    }
}

```

## LockSupport

```


```
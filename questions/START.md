Questions
1. Let’s examine a simple program, “loop.s”. First, just read and un-
derstand it. Then, run it with these arguments (./x86.py -t 1
-p loop.s -i 100 -R dx) This specifies a single thread, an in-
terrupt every 100 instructions, and tracing of register %dx. What
will %dxbe during the run? Use the -cflag to check your answers;
the answers, on the left, show the value of the register (or memory
value) after the instruction on the right has run.

2. Same code, different flags: (./x86.py -p loop.s -t 2 -i 100
-a dx=3,dx=3 -R dx) This specifies two threads, and initializes
each %dx to 3. What values will %dx see? Run with -c to check.
Does the presence of multiple threads affect your calculations? Is
there a race in this code?

3. Run this: ./x86.py -p loop.s -t 2 -i 3 -r -R dx -a
dx=3,dx=3 This makes the interrupt interval small/random; use
different seeds (-s) to see different interleavings. Does the inter-
rupt frequency change anything?

4. Now, a different program, looping-race-nolock.s, which ac-
cesses a shared variable located at address 2000; we’ll call this vari-
able value. Run it with a single thread to confirm your under-
standing: ./x86.py -p looping-race-nolock.s -t 1 -M
2000What is value(i.e., at memory address 2000) throughout the
run? Use -cto check.

5. Run with multiple iterations/threads: ./x86.py -p
looping-race-nolock.s -t 2 -a bx=3 -M 2000Why does
each thread loop three times? What is final value of value?

6. Run with random interrupt intervals: ./x86.py -p
looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 0with
different seeds (-s 1, -s 2, etc.) Can you tell by looking at the
thread interleaving what the final value of valuewill be? Does the
timing of the interrupt matter? Where can it safely occur? Where
not? In other words, where is the critical section exactly?

7. Now examine fixed interrupt intervals: ./x86.py -p
looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 1What
will the final value of the shared variable value be? What about
when you change -i 2, -i 3, etc.? For which interrupt intervals
does the program give the “correct” answer?

8. Run the same for more loops (e.g., set -a bx=100). What inter-
rupt intervals (-i) lead to a correct outcome? Which intervals are
surprising?

9. One last program: wait-for-me.s. Run: ./x86.py -p
wait-for-me.s -a ax=1,ax=0 -R ax -M 2000This sets the
%axregister to 1 for thread 0, and 0 for thread 1, and watches %ax
and memory location 2000. How should the code behave? How is
the value at location 2000 being used by the threads? What will its
final value be?

10. Now switch the inputs: ./x86.py -p wait-for-me.s -a
ax=0,ax=1 -R ax -M 2000How do the threads behave? What
is thread 0 doing? How would changing the interrupt interval (e.g.,
-i 1000, or perhaps to use random intervals) change the trace out-
come? Is the program efficiently using the CPU?
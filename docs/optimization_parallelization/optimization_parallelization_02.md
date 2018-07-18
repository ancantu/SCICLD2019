# Monitoring Jobs

Being able to accurately monitor a job is an invaluable skill.
It will enable you to measure how fast your analyses run, and help track down bugs when issues arise (they often do).

Lets begin by requesting a compute node for 3 hours (180 minutes).

```
$ idev -m 180 -p skx-normal -r LF_18_WEDNESDAY -N 1 -n 48
```

### Runtime

Whenever you run a large task, or want to compare the runtime of programs, the `time` command is the easiest way to track the runtime without editing any source code.

Lets give it a try.

```
$ time ls
```

After listing all of your directories, you should see some text that looks like this.

```
real    0m0.006s
user    0m0.000s
sys     0m0.000s
```

Explanation of values

| Row | Definition |
|-----|------------|
| real | Elapsed walltime |
| user | Time spent in user mode |
| sys | Time spend in kernel mode |

The real time will be the most important for us, and maps to the "walltime" of your jobs. Now that we know how to interpret it, let's try running a longer task.

First, copy a BED file

```
$ cp /work/03076/gzynda/stampede2/ctls-public/SRR2014925.bed .
```

and then sort it, and print out the first few records.

```
$ time sort -S 100M -k1,1 -k2,2n SRR2014925.bed | head
```

Lets try again using our `LC_ALL=C` trick.

```
$ time LC_ALL=C sort -S 100M -k1,1 -k2,2n SRR2014925.bed | head
```

By default, time prints

```
"real %f\nuser %f\nsys %f\n"
```

but we can also print verbose statistics by calling it directly with the `-v` argument

```
$ LC_ALL=C /usr/bin/time -v sort -S 100M -k1,1 -k2,2n SRR2014925.bed | head
```

Which metrics are most interesting?

#### Explore (5 minutes)

- Try timing some other commands you know

### Interactive Monitoring

Sometimes you run a longer pipeline with multiple programs and you want to see how each process is running instead of summary statistics at the end.
The `top` program is a good way to monitor **currently** running tasks.

Open a second terminal to Stampede 2 and `ssh` to your idev node.
**DO NOT** issue another idev command.

Your prompts should show the same host on both terminals.

In your second terminal, launch `top`.
This shows you all running processes on the system.
You can quit top by simply hitting `Q`.

To make your screen less confusing, you can also view just *your* processes with

```
$ top -u [username]
```

Now that you can easily pick out your tasks, lets monitor our un-optimized sort and see what it looks like.

```
$ sort -S 100M -k1,1 -k2,2n SRR2014925.bed | head
```

While inside top, you can also sort by

- Memory (hit `f` then `n` then `Enter`)
- CPU (hit `f` then `k` then `Enter`)
- PID (hit `f` then `a` then `Enter`)

I recommend using top to monitor the following things when using a tool for the first time, so you can answer the following questions:

- How many cores does it use?
  - Each 100% corresponds to a core or thread
- How many programs run at the same time?
  - Watch to make sure you are not over saturating the node with processes
- How much memory is being used?
  - When a program fails, I will often watch it to make sure it never gets close to 100% memory usage.

#### Explore (5 minutes)

- Try monitoring other commands or tools you know
  - tophat2
  - cufflinks
  - blast
<br>
<br>

[Back - Introduction](optimization_parallelization_01.md)
&nbsp;&nbsp;&#151;&nbsp;&nbsp;
[Next - Parallelization](optimization_parallelization_03.md)

# Memory Debugging

[valgrind]: https://valgrind.org/
[memcheck-doc]: https://valgrind.org/docs/manual/mc-manual.html
[massif-doc]: https://valgrind.org/docs/manual/ms-manual.html

A memory debugging is the process of tracking bugs related to the allocation and
deallocation of dynamic memory. This page will start with a short introduction
on how to use Valgrind for this purpose. Then, we present how to use it for a
parallel code.

## Valgrind

The [Valgrind][valgrind] memcheck tool is a tool for memory debugging and 
memory leak detection. Valgrind monitor accesses, allocations, and deallocation
of memory and allows you to debug the following problems:

- bad memory accesses
- uses of uninitialized values
- memory leaks
- double frees or mismatched frees
- overlapping source and destination memory blocks to memory copy functions

To use Valgrind, compile you application with the debug flag (`-g`) so that
Valgrind so that Valgrind can point to you the faulty code lines.

```
cc -g -o my_app source.c
```

Next, run your application with Valgrind 

```
valgrind --tool=memcheck --leak-check=yes ./my_app
```

!!! warning
    When using Valgrind, your application will run much slower than normal and
    use more memory. Take it into consideration when you setup your job.

Valgrind also include a heap profiler named `massif`. This tool measures how 
much heap memory your program uses and can be invoked with the following
command

```
valgrind --tool=massif ./my_app
```

See also:

- Valgrind memcheck [documentation][memcheck-doc]
- Valgrind massif [documentation][massif-doc]

### Valgrind4HPC

Debugging serial applications with Valgrind is quite straightforward, for a 
parallel application with multiple ranks, the output can become messy. That's 
where Valgrind4hpc comes in handy.

Valgrind4hpc aggregates any duplicate messages across ranks to help provide 
an understandable picture of program behaviour. Valgrind4hpc manages starting 
and redirecting output from many copies of Valgrind, as well as deduplicating 
and filtering Valgrind messages.

To use Valgrind4hpc you have to load the module

```
module load valgrind4hpc
```

The next step is to launch your application with Valgrind4hpc. It will take care
of running you application through Slurm. The general form of the command is the
following

```
valgrind4hpc <valgrind4hpc options> <program-name> -- arg1 arg2
```

If your application does not take arguments, the `--` at the end is optional.
The table below summarized the basic options that you can use with Valgrind4hpc.

| Option                        | Description
|-------------------------------|------------------------------------------------|
| `--num-ranks=<num ranks>`     | Specify the number of ranks to run             |
| `--launcher-args=<arguments>` | Arguments to the application launcher (`srun`) |
| `--valgrind-args=<arguments>` | Specify Valgrind arguments                     |
| `--from-ranks=<ranks>`        | Only show Valgrind output from certain ranks   |

For example, to run your application on 16 rank on two nodes, the command will 
be

```
valgrind4hpc --num-ranks=16 \
             --launcher-args="--account=<project> -N2 --ntasks-per-node=8" \
             --valgrind-args="--leak-check=full" \
             ./my_application -- <args>
```



# MultiWrapper

## Installation
```
git clone https://github.com/sdorkenw/MultiWrapper.git
cd MultiWrapper
pip install . --upgrade
```

## Usage

`multiprocessing_utils.py` contains functionality to execute functions (that do not need to communicate with 
each other) in parallel, i.e. the same functions gets called with different parameter sets.

The function `func` needs to accept an array of arguments only. Example:

```
def _add_layer_thread(args):
    dev_mode, layer_id, chunk_coords = args

    cg = chunkedgraph.ChunkedGraph(dev_mode=dev_mode)
    cg.add_layer(layer_id, chunk_coords)
```
Hence `args` needs to be a list of lists - number of jobs x number of arguments. 

There are three ways to start processes in `multiprocessing_utils.py`. The standard approach uses Python's 
`multiprocessing.pool` to start multiple processes or threads, the other creates `subprocesses` via the command line. In general, the former approach should be chosen if there are no significant reasons to choose 'subprocesses' (such as 
library limitations). 

The functions can be called with

```
multiprocessing_utils.multiprocess_func(func, args, n_threads=n_threads)

```

```
multiprocessing_utils.multithread_func(func, args, n_threads=n_threads)

```
 

```
multiprocessing_utils.multisubprocess_func(func, args, n_threads=n_threads)
```

To use `multisubprocess_func` the targeted modules have to be installed in the python environment/
The current implementation waits until all jobs have finished (using `pool.close()` and `pool.join()` or `p.wait()`).

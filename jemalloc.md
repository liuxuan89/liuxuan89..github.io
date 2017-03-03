#### build
`wget https://github.com/jemalloc/jemalloc/releases/download/4.3.1/jemalloc-4.3.1.tar.bz2`  
`tar xvjf jemalloc-4.3.1.tar.bz2`  
`cd jemalloc-4.3.1`  
`./configure --prefix=/lib/jemalloc_profile/ --enable-prof`  
进行heap profile需要编译时--enable-prof  
`make && make install`  
#### profile
```
//....
#include <jemalloc/jemalloc.h>

//....

int main()
{
    //....
    
    {
        const char *fileName = "heap_info_1.out";
        mallctl("prof.dump", NULL, NULL, &fileName, sizeof(const char *));
    }
    return 0;
}
```
`compile with -ljemalloc`
##### Heap Profiling  
@see https://github.com/jemalloc/jemalloc/wiki/Use-Case:-Heap-Profiling  
`MALLOC_CONF="prof:true,prof_prefix:jeprof.out" LD_PRELOAD=/usr/local/lib/libjemalloc.so ./demo`
##### Leak Checking
@see https://github.com/jemalloc/jemalloc/wiki/Use-Case%3A-Leak-Checking  
`MALLOC_CONF="prof:true,prof_prefix:jeprof.out" LD_PRELOAD=/usr/local/lib/libjemalloc.so ./demo`
`jeprof ./demo jeprof.12908.0.f.heap`  
上面的程序名./demo和文件名jeprof.12908.0.f.heap需要根据实际情况修改。
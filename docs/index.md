# Android Open Source Project

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Code Annotation Examples

### Codeblocks

some `code` goes here

### Plain codeblocks

Time and Date in kernel loadable module in Andorid above 5.10.xx version:

```
#include <linux/init.h>
#include <linux/module.h>
#include <linux/ktime.h>
#include <linux/timekeeping.h>
#include <linux/time64.h>
#include <linux/printk.h>
#include <uapi/linux/time.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Ilaveni Ranjith");
MODULE_DESCRIPTION("Kernel module to print current time");

extern void ktime_get_real_ts64(struct timespec64 *tv);

static int hello_init(void)
{	
	time64_t local_time;
	struct tm tm;
	struct timespec64 ts;
	char time[50];
	
	ktime_get_real_ts64(&ts);
	local_time = ts.tv_sec - ( sys_tz.tz_minuteswest * 60);
	time64_to_tm(local_time, 0, &tm);
	
	sprintf(time, "%d-%02d-%02d %02d:%02d:%02d", tm.tm_year + 1900, tm.tm_mon + 1, tm.tm_mday,(int) tm.tm_hour,(int) tm.tm_min,(int) tm.tm_sec );
	
	pr_info("Current: Date & time: %s\n", time);
	
	return 0;
}

static void hello_exit(void)
{
    pr_info("Goodbye Kernel World\n");
}

module_init(hello_init);

```


### code for python language

code with `py` at the start

```py title="graph.py" linenums="1" hl_lines="2 3"
#!/usr/bin/env python
# Convert AOSP build include egrep output to visual graph via graphviz
# ---
# In aosp/build/make/, run something like:
#  egrep -R '(inherit|include)' target/ | $DEV/tools/graph-bulid-includes.py | dot -Tpng > build-includes.png

import re
import sys

lines = sys.stdin.readlines()

nodes = {}
pairs = []
for l in lines:
    l = l.replace("$(SRC_TARGET_DIR)", "target")
    l = l.replace("build/make/target", "target")
    m = re.match('^([-\.\w/_]+):(([\s ,-]|include|\$\(call|inherit|product|if|exists))*([-\.\w/_]+\.mk)\)?', l)
    if m:
        mfile = m.group(1)
        minclude = m.group(4)
        for k in (mfile,minclude):
            if not k in nodes:
                nodes[k] = len(nodes)
                pairs.append((mfile,minclude))
            else:
                sys.stderr.write('Dropped line: {}\n'.format(l))

print("strict digraph includes {")
print("\trankdir=LR")
for (k,v) in nodes.items():
    print("\tn{} [label=\"{}\" shape=box]".format(v, k))

for (mfile,minclude) in pairs:
    print("\tn{} -> n{}").format(nodes[mfile], nodes[minclude])

print("}")
```

## ICons and Emojis

:smile:

:fontawesome-regular-face-laugh-wink:

:fontawesome-brands-facebook:{ .facebook }

:octicons-heart-fill-24:{ .heart }

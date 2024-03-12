# Double Fetch
OS fetch twice the same data from user space to kernel space.
- 1st fetch: check the data is valid
- (context switch)
- 2nd fetch: copy the data to kernel space
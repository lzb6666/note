## CAS2.0
CAS提供了更复杂的问题的解决方案，比如代理模式；
### 代理
考虑CAS1.0的A、B服务，假设A服务需要调用B服务，但是A服务没有该用户访问B服
务的ST，这种情况下A服务可以代替用户向CAS申请一个B服务的ST，因此成为代理。
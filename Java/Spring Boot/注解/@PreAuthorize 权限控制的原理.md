# @PreAuthorize 权限控制的原理

@PreAuthorize 注解，顾名思义是进入方法前的权限验证，@PreAuthorize 声明这个方法所需要的权限表达式，例如：@PreAuthorize("hasAuthority('sys:dept:delete')")，

根据这个注解所需要的权限，再和当前登录的用户角色所拥有的权限对比，如果用户的角色权限集Set中有这个权限，则放行；没有，拒绝

‍

# 渲染顺序问题（刷新）
`utils/request`: export default service AJAX. 若 log out, 触发 store resetToken 清空 token, 但是 service 里面的 token 仍然存在, 会导致刷新页面后, 依然会发送请求, 但是请求的 token 为空, 

`asyncRoute` 先被过滤一遍，发生在后端之后
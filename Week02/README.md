# 作业

问题：我们在数据库操作的时候，比如 dao 层中当遇到一个 sql.ErrNoRows 的时候，是否应该 Wrap 这个 error，抛给上层。为什么，应该怎么做请写出代码

答：是否Wrap这个error应该取决于业务逻辑，如果业务逻辑认为记录不存在是一个错误则需要Wrap，否则甚至可以吞掉这个错误不向上传递

```go
func QueryUser(uid string) (name string, err error) {
    err = db.QueryRow("SELECT name FROM users WHERE id = ?", uid).Scan(&name)
    if err != nil {
        err = errors.Wrap(err, fmt.Sprintf("QueryUser %s error", uid))
        return
    }
    return
}
```
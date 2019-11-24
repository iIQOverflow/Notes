# @ModelAttribute VS @RequestBody
`@ModelAttribute`与@`RequestBody`都是用来注解解析前端发来数据，并自动对应到所定义的字段名称。
## ModelAttribute
使用`@ModelAttribute`注解的实体类接收前端发来的数据格式需要为`"x-www-form-urlencoded"`。
## RequestBody
`@RequestBody`注解的实体类接收前端的数据格式为`JSON(application/json)`格式。(若是使用`@ModelAttribute`接收`application/json`格式，虽然不会报错，但是值并不会自动填入)
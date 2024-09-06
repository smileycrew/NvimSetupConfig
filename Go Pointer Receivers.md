Description - A function can be associated with specific types using method receivers parameters.

My interpretation - I can call a .function() method on a instance.

Example

```go
// Render - renders a named template using the provided data and writes the result to the io.Writer
func (template *Template) Render(writer io.Writer, name string, data interface{}) error {
    return template.templates.ExecuteTemplate(writer, name, data)
}
```

This function is using a reference of the Template type, which is a custom type of the html/template library.

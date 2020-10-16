# preface
you can use textarea, codemirror, etc...
but these are different each other.
and maybe you have something else that wish.


# Vue2-Editable
it almost alike textarea tag.
if you want to apply contenteditable to Vue 2, it is not bad alternative.



## how to use?
```html
<Editable
    class="contents"
    :text.sync="text" (reactive data)
    :maxlength="260"
    :input
    :keyup
    :keydown
    :focus
    :blur
    :plaintext (true | false, if it true, change HTML code to plaintext when you pasting)
/>
```

# notes
this is extremely lightweight and no room for updates.
its mean that not worth adding to dependencies, so i didnt add to npm.
all you have to do is add the code as your component.

# support
vue@2.3 ~ 3
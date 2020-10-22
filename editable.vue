<template>
<div ref="editable"
     contenteditable="true"
     @input="_input"
     @keyup="_keyup"
     @keydown="_keydown"
     @focus="_focus"
     @blur ="_blur"
     @paste="_paste"></div>
</template>
<script>
export default {
    props: [
        'text',

        'input',
        'keyup',
        'keydown',

        'focus',
        'blur',

        'maxlength',
        'plaintext'
    ],
    data() {
        return {
            isFocused: false,
        }
    },
    methods: {
        _input(e) {
            this.input ? this.input(e) : 0;
            const text = this.$refs.editable.innerText;
            this.$emit('update:text', text)
        },
        async _keyup(e) {
            this.keyup ? this.keyup(e) : 0;
            
            
            function setCursorAtEnd(el) {
                if(!window.getSelection || !document.createRange) return;
                var range = document.createRange();
                var selection = window.getSelection();
                range.setStart(el.childNodes[0], this.count);
                range.collapse(true);
                selection.removeAllRanges();
                selection.addRange(range);
                el.focus();
            }
            
            if(this.count > this.maxlength) {
                const el = this.$refs.editable;
                this.isFocused = false;
                const text = this.text.substring(0, this.maxlength);
                await this.$emit('update:text', text)
                this.isFocused = true;
                this.setCursorAtEnd(el);
            }
        },
        _keydown(e) {
            this.keydown ? this.keydown(e) : 0;
            if(this.count > this.maxlength) {
                e.preventDefault();
                this.isFocused = true;
            }
        },
        _focus(e) {
            this.focus ? this.focus(e) : 0;
            this.isFocused = true;
        },
        _blur(e) {
            this.blur ? this.blur(e) : 0;
            this.isFocused = false;
        },
        
        _paste(e) {
            e.preventDefault();
            this.paste ? this.paste(e) : 0;

            function HTMLtoPlainTextWhenPaste() {
                e.preventDefault();
                // get text representation of clipboard
                var text = (e.originalEvent || e).clipboardData.getData('text/plain');
                // insert text manually
                document.execCommand("insertHTML", false, text);
            }
            if(this.plaintext) HTMLtoPlainTextWhenPaste();
        }
    },

    watch: {
        text(newVal) {
            if(!this.isFocused)
                this.$refs.editable.innerHTML = newVal || '';
        },
        isFocused(newVal) {
            if(newVal) this.$refs.editable.focus();
            else       this.$refs.editable.blur();
        }
    },

    mounted() {
        this.$refs.editable.innerHTML = this.text || '';
    }
}
</script>

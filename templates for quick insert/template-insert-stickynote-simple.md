<%*
let editor = app.workspace.activeEditor.editor;
let cur = editor.getCursor();
editor.replaceRange('💬', cur);
editor.setCursor({line: cur["line"], ch: 0});
_%>> [!stickynote]
> <% tp.file.cursor() %>

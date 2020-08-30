# 
- TicTacToe
    + element.classList.add()/remove()：向元素的类列表中增加/移除类
    + inline-block默认的对齐方式是baseline，设置vertical-align为middle，才能使其排列整齐
    + 给元素添加与父元素高度一样的line-height，能让文字垂直居中
    + 添加换行：element.appendChild(document.createElement("br"))
    + 数组深拷贝：JSON.parse(JSON.stringify(array))
    + 如果数组是一维数组，可以Object.create(array)方法使用深拷贝

- mapEditor
    + mousedown事件：在指针设备按下时触发.event.which：左键报告1，中间键报告2，右键报告3
    + contextmenu事件：在用户尝试打开上下文菜单时被触发，通过调用事件的preventDefault()方法来阻止弹出菜单
    
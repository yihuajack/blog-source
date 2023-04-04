---
title: Gtk Unable to load resource for composite template
categories: Programming Language
tags:
  - ui
  - gtk
  - gresource
date: 2023-02-24 18:08:23
---

在使用 GNOME Builder 构建应用时，设 gnome_semilab_window 为应用主窗口，实现在 gnome-semilab-window.c 中；设 gsp_create_project_widget 为主窗口上的一个子组件，实现在 gsp-create-project-widget.c 中，插入到主窗口的 GUI 中。两个模块各自使用了 GtkBuilder XML UI 文件。在子组件文件中，定义子组件的类初始化函数：
```c
static void
gsp_create_project_widget_class_init (GspCreateProjectWidgetClass *klass)
{
  GtkWidgetClass *widget_class = GTK_WIDGET_CLASS (klass);

  gtk_widget_class_set_template_from_resource (widget_class, "gsp-create-project-widget.ui");
  gtk_widget_class_bind_template_child (widget_class, GspCreateProjectWidget, expand_button);

  gtk_widget_class_install_action (widget_class, "create-project.expand", NULL, expand_action);
}
```
报错：
> (gnome-semilab:2): Gtk-CRITICAL **: 18:00:38.011: Unable to load resource for composite template for type 'GspCreateProjectWidget': The resource at “gsp-create-project-widget.ui” does not exist

> (gnome-semilab:2): Gtk-CRITICAL **: 18:00:38.011: gtk_widget_class_bind_template_child_full: assertion 'widget_class->priv->template != NULL' failed

> (gnome-semilab:2): Gtk-CRITICAL **: 18:00:38.011: gtk_widget_init_template: assertion 'template != NULL' failed

参考 StackOverflow 唯一相关问题 [Unable to load resource for composite template](https://stackoverflow.com/questions/43065437/unable-to-load-resource-for-composite-template)，没有参考性。
经反复学习，得到错误原因是[`gtk_widget_class_set_template_from_resource()`](https://docs.gtk.org/gtk4/class_method.Widget.set_template_from_resource.html)函数的`resource_name`参数必须为资源的绝对路径，例如`/org/gnome/libide-greeter/ide-greeter-row.ui`。修改后错误消失。
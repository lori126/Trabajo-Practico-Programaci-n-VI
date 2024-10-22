import flet as ft

def main(page: ft.Page):
    page.adaptive = True
    page.window_width = 600
    page.window_height = 400
    page.title = "Lista"
    page.vertical_alignment = "center"

    logo = ft.Image(src="logo.png", width=100, height=100)
    logoN = ft.Text(value="Notas", color="white", size=50, weight=ft.FontWeight.BOLD)

    def edit_clicked(e, checkbox):
        new_value = ft.TextField(value=checkbox.label, hint_text="Nuevo valor")
        save_button = ft.ElevatedButton("Guardar", on_click=lambda e: save_clicked(e, checkbox, new_value))
        page.dialog = ft.AlertDialog(content=ft.Column([new_value, save_button], width=300, height=100, alignment=ft.MainAxisAlignment.CENTER))
        page.dialog.open = True
        page.update()

    def save_clicked(e, checkbox, new_value):
        checkbox.label = new_value.value
        page.dialog.open = False
        checkbox.update()
        page.update()

    def delete_clicked(e, row):
        page.controls.remove(row)
        page.update()

    def add_clicked(e):
        if new_task.value.strip():
            checkbox = ft.Checkbox(label=new_task.value)
            row = ft.Row(
                [
                    checkbox,
                    ft.IconButton(icon=ft.icons.EDIT, icon_color="green400", icon_size=20, tooltip="Editar", on_click=lambda e: open_edit_dialog(e, task_checkbox)),
                ft.IconButton(icon=ft.icons.DELETE, icon_color="yellow400", icon_size=20, tooltip="Eliminar", on_click=lambda e: delete_task(e, task_row))
                ],
                alignment="center"
            )
            page.add(row)
            new_task.value = ""
            new_task.focus()
            new_task.update()

    new_task = ft.TextField(hint_text="Añadir una nueva nota.")
    header = ft.Row([logo, logoN], alignment="center")
    
    page.add(
        header,
        ft.Row([ft.Text(value="¡Bienvenido!", size=15)], alignment="center"),
        ft.Divider(height=10),
        ft.Row([new_task, ft.ElevatedButton("Añadir", on_click=add_clicked)], alignment="center")
    )

ft.app(target=main)

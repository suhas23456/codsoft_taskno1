# codsoft_taskno1
#task 1 :To do list app using python

import tkinter as tk
from tkinter import messagebox

class Task:
    def __init__(self, description, status="Incomplete"):
        self.description = description
        self.status = status

class TaskManager:
    def __init__(self):
        self.tasks = []

    def add_task(self, task):
        self.tasks.append(task)

    def update_task_description(self, index, new_description):
        if 0 <= index < len(self.tasks):
            self.tasks[index].description = new_description
        else:
            messagebox.showwarning("Warning", "Invalid task index.")

class ToDoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List App")

        self.task_manager = TaskManager()

        # Task entry
        self.task_entry = tk.Entry(root, width=40)
        self.task_entry.pack(pady=10)

        # Add Task button
        add_task_button = tk.Button(root, text="Add Task", command=self.add_task, bg="green", fg="white")
        add_task_button.pack(pady=5)

        # Task Listbox
        self.task_listbox = tk.Listbox(root, selectmode=tk.SINGLE, height=10, width=40, font=("Arial", 14))
        self.task_listbox.pack(pady=10)

        # Mark as Complete button
        mark_complete_button = tk.Button(root, text="Mark as Complete", command=self.mark_as_complete, bg="blue", fg="white")
        mark_complete_button.pack(pady=5)

        # Update button
        update_button = tk.Button(root, text="Update", command=self.update_task, bg="blue", fg="white")
        update_button.pack(pady=5)

        # Exit button
        exit_button = tk.Button(root, text="Exit", command=root.destroy, bg="red", fg="white")
        exit_button.pack(pady=5)

    def add_task(self):
        task_description = self.task_entry.get()
        if task_description:
            task = Task(description=task_description)
            self.task_manager.add_task(task)
            self.update_task_listbox()
            self.task_entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Warning", "Task description cannot be empty.")

    def update_task_listbox(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.task_manager.tasks:
            status = "[Complete]" if task.status == "Complete" else "[Incomplete]"
            self.task_listbox.insert(tk.END, f"{status} {task.description}")

    def mark_as_complete(self):
        selected_task_index = self.task_listbox.curselection()
        if selected_task_index:
            task_index = selected_task_index[0]
            if self.task_manager.tasks[task_index].status == "Incomplete":
                self.task_manager.tasks[task_index].status = "Complete"
                self.update_task_listbox()
            else:
                messagebox.showwarning("Warning", "Task is already marked as complete.")
        else:
            messagebox.showwarning("Warning", "Please select a task.")

    def update_task(self):
        new_description = self.task_entry.get()
        selected_task_index = self.task_listbox.curselection()
        if selected_task_index:
            task_index = selected_task_index[0]
            self.task_manager.update_task_description(task_index, new_description)
            self.update_task_listbox()
            self.task_entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Warning", "Please select a task.")

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoApp(root)
    root.mainloop()

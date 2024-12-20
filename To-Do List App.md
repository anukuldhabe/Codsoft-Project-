# Codsoft-Project-
import json
from datetime import datetime

# File to store tasks
TASK_FILE = "tasks.json"

# Load tasks from file
def load_tasks():
    try:
        with open(TASK_FILE, "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return []


# Save tasks to file
def save_tasks(tasks):
    with open(TASK_FILE, "w") as file:
        json.dump(tasks, file, indent=4)


# Display tasks
def display_tasks(tasks):
    if not tasks:
        print("\nNo tasks found.")
        return
    print("\nYour Tasks:")
    for i, task in enumerate(tasks, start=1):
        status = "✓" if task["status"] == "completed" else "✗"
        print(f"{i}. {task['title']} ({task['priority']} priority) - Due: {task['due_date']} [{status}]")
        if task["description"]:
            print(f"   Description: {task['description']}")


# Add a task
def add_task(tasks):
    title = input("Enter task title: ").strip()
    description = input("Enter task description (optional): ").strip()
    due_date = input("Enter due date (YYYY-MM-DD): ").strip()
    priority = input("Enter priority (High, Medium, Low): ").strip()
    task = {
        "title": title,
        "description": description,
        "due_date": due_date,
        "priority": priority,
        "status": "pending"
    }
    tasks.append(task)
    save_tasks(tasks)
    print("Task added successfully!")


# Mark a task as completed
def mark_task_completed(tasks):
    display_tasks(tasks)
    if not tasks:
        return
    try:
        task_index = int(input("\nEnter the task number to mark as completed: ")) - 1
        if 0 <= task_index < len(tasks):
            tasks[task_index]["status"] = "completed"
            save_tasks(tasks)
            print("Task marked as completed!")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")


# Delete a task
def delete_task(tasks):
    display_tasks(tasks)
    if not tasks:
        return
    try:
        task_index = int(input("\nEnter the task number to delete: ")) - 1
        if 0 <= task_index < len(tasks):
            tasks.pop(task_index)
            save_tasks(tasks)
            print("Task deleted successfully!")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Please enter a valid number.")


# Main menu
def main():
    tasks = load_tasks()
    while True:
        print("\nTo-Do List Manager")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Mark Task as Completed")
        print("4. Delete Task")
        print("5. Exit")
        choice = input("Enter your choice: ").strip()
        
        if choice == "1":
            display_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            mark_task_completed(tasks)
        elif choice == "4":
            delete_task(tasks)
        elif choice == "5":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()

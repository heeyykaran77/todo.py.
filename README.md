# todo.py.
# A simple command-line to-do list application

import sys
import os

TODO_FILE = "todo.txt"

def show_help():
    """Prints the help message."""
    print("\n--- To-Do List Manager ---")
    print("Usage: python todo.py [command] [argument]")
    print("Commands:")
    print("  add 'task'  - Adds a new task to the list.")
    print("  list        - Lists all tasks.")
    print("  done <index> - Marks a task as done (removes it).")
    print("  help        - Shows this help message.")
    print("--------------------------\n")

def add_task(task):
    """Adds a new task to the todo.txt file."""
    with open(TODO_FILE, "a") as f:
        f.write(task + "\n")
    print(f"Added task: '{task}'")

def list_tasks():
    """Lists all tasks from the todo.txt file."""
    if not os.path.exists(TODO_FILE) or os.path.getsize(TODO_FILE) == 0:
        print("No tasks found. Add one with 'add'!")
        return

    print("\n--- Your Tasks ---")
    with open(TODO_FILE, "r") as f:
        tasks = f.readlines()
        if not tasks:
            print("Your to-do list is empty.")
        else:
            for i, task in enumerate(tasks, 1):
                print(f"{i}. {task.strip()}")
    print("------------------\n")


def complete_task(task_index):
    """Removes a task by its index."""
    try:
        index = int(task_index)
        with open(TODO_FILE, "r") as f:
            tasks = f.readlines()

        if 1 <= index <= len(tasks):
            removed_task = tasks.pop(index - 1).strip()
            with open(TODO_FILE, "w") as f:
                f.writelines(tasks)
            print(f"Completed task: '{removed_task}'")
        else:
            print("Error: Invalid task index.")
            list_tasks()
    except ValueError:
        print("Error: Please provide a valid number for the task index.")
    except FileNotFoundError:
        print("No tasks to complete.")

def main():
    """Main function to handle command-line arguments."""
    if len(sys.argv) < 2:
        show_help()
        return

    command = sys.argv[1].lower()

    if command == "add":
        if len(sys.argv) > 2:
            add_task(" ".join(sys.argv[2:]))
        else:
            print("Error: Missing task description for 'add' command.")
    elif command == "list":
        list_tasks()
    elif command == "done":
        if len(sys.argv) > 2:
            complete_task(sys.argv[2])
        else:
            print("Error: Missing task index for 'done' command.")
    elif command == "help":
        show_help()
    else:
        print(f"Error: Unknown command '{command}'")
        show_help()

if _name_ == "_main_":
    main()

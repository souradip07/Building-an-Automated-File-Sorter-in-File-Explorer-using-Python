# Building-an-Automated-File-Sorter-in-File-Explorer-using-Python
import os
import shutil

def create_folder_if_not_exists(folder_path):
    """Create a folder if it does not exist."""
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)

def sort_files(directory):
    """Sort files in the specified directory into subfolders by extension."""
    if not os.path.exists(directory):
        print(f"The directory '{directory}' does not exist.")
        return
    
    # Mapping of extensions to folder names
    file_categories = {
        "Images": [".jpg", ".jpeg", ".png", ".gif", ".bmp"],
        "Documents": [".pdf", ".doc", ".docx", ".txt", ".xlsx", ".pptx"],
        "Videos": [".mp4", ".mkv", ".avi", ".mov"],
        "Audio": [".mp3", ".wav", ".aac"],
        "Archives": [".zip", ".rar", ".tar", ".gz"],
        "Others": []
    }

    # Sort files
    for filename in os.listdir(directory):
        file_path = os.path.join(directory, filename)
        
        # Skip directories
        if os.path.isdir(file_path):
            continue
        
        file_extension = os.path.splitext(filename)[1].lower()
        sorted = False

        # Check which category the file belongs to
        for category, extensions in file_categories.items():
            if file_extension in extensions:
                category_folder = os.path.join(directory, category)
                create_folder_if_not_exists(category_folder)
                shutil.move(file_path, os.path.join(category_folder, filename))
                sorted = True
                break
        
        # Handle files with unlisted extensions
        if not sorted:
            others_folder = os.path.join(directory, "Others")
            create_folder_if_not_exists(others_folder)
            shutil.move(file_path, os.path.join(others_folder, filename))
    
    print("Files sorted successfully!")

# Specify the directory you want to organize
target_directory = input("Enter the path of the directory to sort: ").strip()
sort_files(target_directory)

# Python-Scripting-Project

"This project is a build automation tool I created in Python to handle multiple Go-based game projects. It scans a source directory for all subdirectories with ‘game’ in their name, copies them to a target location, compiles the Go source files using go build, and generates a metadata.json to summarize all processed games. It also handles cleanup and renaming of directories to remove _game suffixes. I implemented modular functions for readability and maintainability—each with a single responsibility—and used the subprocess module to safely run build commands. This would be particularly useful in CI/CD pipelines or local automation setups where multiple game projects need to be built and packaged consistently."

✅ Overview: What This Code Does
This Python script automates the process of:

1.Finding all game project directories in a given source directory.

2.Copying each game directory to a new target location (with cleaned names).

3.Compiling Go (.go) source code inside each copied game directory.

4.Generating a metadata JSON file summarizing all processed games.

This is useful for managing multiple Go-based game projects—for example, organizing and building them in preparation for deployment, testing, or packaging.

🧱 Key Components and Their Responsibilities
1. Constants
~~~
python
Copy
Edit
GAME_DIR_PATTERN = "game"
GAME_CODE_EXTENSION = ".go"
GAME_COMPILE_COMMAND = ["go", "build"]
~~~
These are configuration values:

-> Search for directories containing “game” in their names.

-> Look for Go source files (.go).

-> Compile them using go build.

2. Function:
   ~~~
    find_all_game_paths(source)
   ~~~

-> Purpose: Scans the given source directory for subdirectories with names containing “game”.

-> Returns: A list of absolute paths of these "game" directories.

✅ Note: The break ensures only top-level directories are scanned (non-recursive).

3. Function:
   ~~~
    get_name_from_paths(paths, to_strip)
   ~~~
-> Purpose: Cleans up directory names by removing _game from each.

-> E.g., 'space_game' → 'space'.

-> Returns: List of cleaned names.

4. Function:
~~~
 create_dir(path)
~~~
-> Purpose: Creates the target directory if it doesn’t already exist.

5. Function:
   ~~~
    copy_and_overwrite(source, dest)
   ~~~
-> Purpose: Copies the game directory to a new location.

-> If the destination already exists, it’s deleted and replaced.

6. Function:
~~~
   make_json_metadata_file(path, game_dirs)
~~~
-> Purpose: Generates a metadata.json file in the target directory.

Contents:
~~~
json
Copy
Edit
{
    "gameNames": ["space", "puzzle", "maze"],
    "numberOfGames": 3
}
~~~
7. Function:
   ~~~
   run_command(command, path)
   ~~~
-> Purpose: Runs a shell command (like go build) inside a given directory.

-> Captures and prints the output (stdout/stderr).

8. Function:
   ~~~
   compile_game_code(path)
   ~~~
-> Purpose: Locates the first .go file in a game directory.

-> Then compiles it using go build <filename>.

-> Only searches the top-level files, not recursively.

9. Function:
~~~
 main(source, target)
~~~
This is the core execution logic:

   1.Gets full paths of source and target.

   2.Finds all game directories in the source.

   3.Cleans up the names.

   4.Creates the target directory.

   5.For each game:

     ->Copies it to the new location.

   ->Compiles its .go file.

   6.Writes metadata JSON in the target folder.

11. ~~~
    if __name__ == "__main__"
    ~~~
    Block
-> Purpose: Ensures the script runs only when called from the command line.

-> Requires: Two command-line arguments — source and target directories.

To run this code use:
~~~
python get_game_data.py data target
~~~



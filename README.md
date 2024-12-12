# memwrap

`memwrap` is a wrapper script for Linux that launches a command with a hard memory limit. It helps to kill the command if it crosses a memory threshold, making it useful for Cloud VMs.

## Usage

```sh
memwrap <command>
```

Example with environment variables:

```sh
env MEMWRAP_LIMIT=100 MEMWRAP_POLLING=1 memwrap <command>
```

## Available Environment Variables

- `MEMWRAP_LIMIT`:
  - **Description**: Controls the max memory limit the command can use.
  - **Default**: 500
  - **Value**: Value in MB

- `MEMWRAP_POLLING`:
  - **Description**: Controls the sleep time to check for process info.
  - **Default**: 1 
  - **Value**: Value in seconds

- `MEMWRAP_DEBUG`:
  - **Description**: Prints additional info.
  - **Default**: 0
  - **Value**: 0 or 1

## Example

```sh
# Run a command with a memory limit of 100 MB and polling interval of 1 second
env MEMWRAP_LIMIT=100 MEMWRAP_POLLING=1 memwrap your_command_here
```

## Installation

To use `memwrap`, you need python3 executable in PATH and just run the script directly. Alternatively one can move the script to their /usr/local/bin/

```sh
git clone <memwrap repo>
cd <clone>
./memwrap <command>
```

## Contributing

Feel free to submit issues or pull requests if you have any improvements or bug fixes.

## License

Check the LICENSE file for details


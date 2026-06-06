# Create a New Simulation

Previous: [Installation](installation.md)

## Setup

It is recommended to create each simulation in its own dedicated folder, separate from the SwarmSwIM installation directory (if installed from source).

For this example, create a folder called `mysim`:

```bash 
mkdir mysim && cd mysim
```

If SwarmSwIM was installed inside a virtual environment, make sure the environment is activated first. See [Activate the vitural envrioment](installation.md#activate-the-virtual-envrioment).

Generate a new simulation project using:

```bash
SwarmSwIM create_new
```

You should see:

```
$ Initiated SwarmSwIM env templates in .
```

On some systems, particularly Windows or certain Python environments, the `SwarmSwIM` command may not be available in the terminal.

In that case, you can invoke the CLI directly through Python:

```bash
python -m SwarmSwIM.utility.cli create_new
```

## Run the Example

The project generator creates a ready-to-run example script. Test your installation by running:

```bash
python example.py
```

If everything is configured correctly, the simulator and visualizer should start.

---

Learn how the generated example works: [Analyze the `example.py` script](analyze_example.md).

Continue the tutorial: [Create a new simulation script](write_new_script.md).

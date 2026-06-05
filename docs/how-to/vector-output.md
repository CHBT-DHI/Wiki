---
tags:
  - how-to
  - output
---

# Vector and Wastewater Output

Export entire ASM component vectors from the simulation — useful for linking WEST outputs to downstream models or for detailed mass balance analysis.

**Source**: WEST User Guide §5.6.

---

### Wastewater Output (standard ASM vector)

The **Wastewater Output** block exports the full ASM component vector at a selected point in the layout. This is the reverse of the Wastewater Input: instead of importing bulk parameters, it exports all state variable concentrations.

**Use cases:**
- Pass effluent compositions to a downstream river model
- Check the full component vector at intermediate points (e.g. after primary clarifier)
- Verify mass balances across a unit process

**Setup:**
1. From the Block Library → Input and Output → drag **Effluent** block onto the canvas.
2. Connect to the stream you want to monitor.
3. Double-click → **General** tab → set Model to `Effluent Generator` (standard) or `Custom vector`.
4. In the **De-fractionation** tab, the individual ASM components are listed as outputs.

---

### Vector Data Output

The **Vector Data Output** block exports a user-defined vector of variables to a file at each time step. Unlike scalar Data Output (which exports individual variables), Vector Output exports ordered arrays — useful for interfacing with external tools.

**Setup:**
1. **Insert → Output → Vector Data Output** to place the block.
2. Double-click → define the **variable list** (drag from Model Explorer).
3. Set the **output file path** and **output interval**.
4. The output file is a tab-separated text file with one column per variable.

---

### Scalar Data Output (recap)

For individual variable export, use **Insert → Output → Data Output**:
- Select variables from the Model Explorer
- Set file path (e.g. `C:\Projects\MyProject\output\effluent.txt`)
- Set output interval (e.g. every 0.1 days = 2.4 hours)
- File is written during the run; view in any text editor or import to Excel

---

### Output file formats

| Format | Extension | Notes |
|---|---|---|
| Tab-separated text | `.txt` | Default; compatible with Excel, MATLAB, R |
| CSV | `.csv` | Set in output block properties |
| WEST binary | `.wst_out` | Faster write speed for large models; open with Data Editor |

---

### Related

- [Results & Output](results-and-output.md)
- [Managing Input Data](managing-input-data.md)
- [Effluent De-fractionation](effluent-defractionation.md)

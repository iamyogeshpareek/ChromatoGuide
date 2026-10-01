# ChromatoGuide

**Chemistry-informed HPLC screening and method development.**

## Why I started this project

I wanted a skill-building project that connects analytical chemistry with software. The question that motivates it is straightforward: **can an adaptive, multi-objective, green-AQbD approach reach agreed method targets with fewer experiments than a conventional DoE?**

I added an analyte-first step because method development starts before optimization. Molecular properties can help identify chromatographic modes and column chemistries worth investigating. They cannot tell us in advance which method will work, so ChromatoGuide presents those choices as hypotheses to test.

The project is still evolving. The app helps organize experiments and explore ideas; it has not established that adaptive optimization saves runs. That requires comparable experimental data from both study arms.

## What the app does

- Accepts analyte descriptors or calculates basic 2D descriptors from a SMILES string when RDKit is installed.
- Ranks RP, HILIC, ion-exchange, and mixed-mode starting points, with candidate column chemistries and a visible rationale.
- Reads a time/response CSV and reports candidate chromatogram peaks for review.
- Checks resolution, tailing, plate count, and replicate area %RSD against limits you provide.
- Creates a randomized three-factor Box–Behnken plan and records a separate adaptive study arm.
- Suggests adaptive factor settings, tracks green solvent-volume measures, and compares observed runs against the same user-defined criteria.
- Provides symptom-based troubleshooting prompts and a study overview.

## Design choices I can explain

- **Visible rules first:** mode and column suggestions use simple descriptor rules with a written rationale, rather than a black-box prediction.
- **Compare against a real baseline:** the conventional arm uses a 15-run Box–Behnken design; the adaptive arm begins with six space-filling runs before using recorded responses.
- **Keep the objectives explicit:** the adaptive prototype considers resolution, runtime, and organic-solvent use. It only learns from data entered in the adaptive arm.
- **Show real factor settings:** coded design levels are mapped to the study's organic fraction, pH, and temperature ranges.
- **Keep green claims narrow:** the current green metrics estimate solvent volumes. They are not AGREE, GAPI, or life-cycle assessments.

When presenting the project, I should be able to explain these choices, walk through the code, and distinguish software checks from experimental validation. I should also describe any AI or other tool assistance accurately if asked or if my course or institution requires disclosure.

## Run locally

Use Python 3.10 or newer. From the project folder:

```powershell
py -m pip install -r requirements.txt
py -m streamlit run app.py
```

Open the local address printed by Streamlit. Manual descriptor entry works without RDKit. To calculate descriptors from SMILES, install RDKit in the same Python environment using the [RDKit installation guide](https://www.rdkit.org/new_docs/Install.html). RDKit does not provide pKa, pH-specific logD, solubility, or chromatographic retention in this app.

## Keep and reopen a study

Use **Save complete study project (JSON)** in the AQbD / DoE tab. Use **Open saved study** in the sidebar and **Load study** to continue later. The project file stores entered study settings and results; the original chromatogram CSV is not embedded, so keep that source file separately for traceability. CSV export is also available for the experiment log.

## Scientific limits

ChromatoGuide is a learning and research prototype, not validated analytical software. The peak picker uses simple baseline and noise heuristics; inspect its integrations manually. Suitability limits are examples and must be replaced with the criteria from the applicable method or protocol. Troubleshooting suggestions are rule-based prompts, not automated diagnoses.

The adaptive optimizer uses a fixed-kernel Gaussian-process prototype on a small candidate grid. It has not been validated for a specific chromatographic system. Three passing confirmation injections at one condition are a limited operational check, not proof of method robustness across days, analysts, instruments, or deliberate parameter variation. Use the same analyte, factor ranges, targets, measurement procedure, and replicate policy for both study arms. Do not claim a run reduction until the comparison has been performed prospectively and repeated.

## Next work

1. Compare peak finding with instrument-exported traces and reference integrations; improve baseline handling and preserve links to original files.
2. Let users save method-specific suitability criteria and replicate measurements.
3. Add model diagnostics and test adaptive recommendations on simulated and measured datasets.
4. Run replicated, matched comparisons of conventional and adaptive strategies; report uncertainty and unsuccessful cases as well as successful ones.
5. Add sourced solvent-hazard data before implementing validated green-chemistry scores.

## GitHub upload

Create a repository and upload the project files while preserving the `src/hplc_copilot/` structure. Do not commit private chromatograms, study records, virtual environments, or secrets. The `.gitignore` excludes common local files.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE). Replace `[Your Name]` in that file with the name you want to use as the copyright holder before publishing.

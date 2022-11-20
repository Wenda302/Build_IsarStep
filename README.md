# Build_IsarStep

## For building IsarStep from scratch
1. We may need the following packages:
    ```
        pip install tatsu algebraic-data-types regex
    ```

2. Download a [modified version of Isabelle2022](https://www.dropbox.com/s/lbqjt7p01vj20m2/Isabelle2022_modified.zip?dl=0). The modifications include:
    - [Recording/Base64.sml](isarstep_scripts/Recording/Base64.sml) is moved to `src/Pure/`;
    - [Recording/ProofRecording_Utils.thy](isarstep_scripts/Recording/ProofRecording_Utils.thy) and [Recording/ProofRecording.thy](isarstep_scripts/Recording/ProofRecording.thy) are merged into `src/Pure/Pure.thy`.
    
3. Download the current version of [AFP](https://www.isa-afp.org/download/) and [link](https://www.isa-afp.org/help/) it properly to Isabelle2022: make sure `<path to AFP2022>/thys` is added to `~/.isabelle/Isabelle2022/ROOTS`.

4. Download a lightly pre-processed [isar_dataset](https://www.dropbox.com/s/ro5fqctq3p8s3yh/isar_dataset.zip?dl=0), which includes the current AFP (on Nov. 20, 2022) and the HOL standard library of Isabelle2022. Pre-processing steps include:
    - for each ROOT file, we replace `session xxx` with `session xxx_`.
    - `HOL/ROOT` is slightly tuned.

5. Run the following command 
    ```
        python build_isarstep_database.py --isa_bin <Isabelle binary> --isar_data <isar_dataset>
    ```
    where `<Isabelle binary>` should be a path to the modified Isabelle2022 binary (e.g., /home/wenda/Programs/Isabelle2022_modified/bin/isabelle), and `<isar_dataset>` refers the path to the pre-processed repository in the previous step. This will take about 30-40 hours on a 90-core CPU. At the end, you will get a processed repository (still within `<isar_dataset>`) and a `<processed_id>` from the standard output.

5. Run the following command 
    ```
        python extract_isarstep_from_database.py --dir_in <isar_dataset> --processed_id <processed_id>
    ```
    where `<isar_dataset>` and `<processed_id>` are from previous steps. This will generates a repository named `extracted_isar_dataset` in the current directory, which contains the raw data points of IsarStep.


# Graph Isomorphism Network on Tox21 Dataset
### Task
Multi-task binary toxicity prediction: for each molecule the model outputs 12 probabilities, one for each assay (e.g. NR-AR, SR-ARE, p53, etc.). At training time we use a binary cross-entropy loss (with masking for missing labels) over those 12 tasks, and at the end of each epoch we compute the ROC-AUC per task (then average) on the held-out validation set to see how well the model is distinguishing actives vs. inactives across all assays.

### Tox21 Dataset Description
The **Tox21 Dataset has $12$ assays.**  

---

**Nuclear Receptor (NR) assays (7 endpoints)**

1. **NR-AR** (Androgen Receptor)  
   - **Target**: Full-length human AR  
   - **Cell line**: MDA-kb2 breast cancer cells expressing luciferase under an AR-responsive promoter  
   - **Readout**: Luminescence upon AR agonist binding, measures agonist activity  

2. **NR-AR-LBD** (Androgen Receptor Ligand-Binding Domain)  
   - **Target**: AR ligand-binding domain fused to a transcriptional activator  
   - **Assay format**: β-lactamase reporter in HEK293-derived cells  
   - **Readout**: β-lactamase signal indicating direct LBD binding  

3. **NR-ER** (Estrogen Receptor α)  
   - **Target**: Full-length human ER α  
   - **Cell line**: BG1 ovarian carcinoma cells with ER-responsive luciferase reporter  
   - **Readout**: Luminescence upon estrogen-like agonism  

4. **NR-ER-LBD** (Estrogen Receptor α Ligand-Binding Domain)  
   - **Target**: ER α LBD fused to a DNA-binding domain  
   - **Cell line**: GAL4-ER α LBD reporter in HEK293-derived cells  
   - **Readout**: Reporter activation upon direct LBD engagement  

5. **NR-AhR** (Aryl Hydrocarbon Receptor)  
   - **Target**: Human AhR  
   - **Cell line**: HepG2 cells with DRE-driven luciferase reporter  
   - **Readout**: Luminescence when ligands (e.g. dioxin analogs) activate AhR  

6. **NR-Aromatase**  
   - **Target**: CYP19A1 (Aromatase enzyme)  
   - **Format**: Cell-free or microsomal conversion of testosterone to estradiol  
   - **Readout**: Fluorescent or luminescent detection of estradiol, measures inhibition of estrogen synthesis  

7. **NR-PPAR-γ** (Peroxisome Proliferator-Activated Receptor γ)  
   - **Target**: Human PPAR γ  
   - **Cell line**: PPRE-driven luciferase reporter in CV-1 or HEK293 cells  
   - **Readout**: Reporter activation by PPAR γ agonists  

---

**Stress Response (SR) assays (5 endpoints)**

8. **SR-ARE** (Antioxidant Response Element)  
   - **Pathway**: Nrf2-ARE oxidative stress response  
   - **Cell line**: HepG2 cells with ARE-luciferase reporter  
   - **Readout**: Luminescence when oxidative-stress inducers activate the pathway  

9. **SR-ATAD5** (ATAD5 DNA Damage Response)  
   - **Target**: ATAD5 promoter stability  
   - **Cell line**: HEK293 cells with luciferase-tagged ATAD5  
   - **Readout**: Reporter stabilization upon genotoxic stress  

10. **SR-HSE** (Heat Shock Element)  
    - **Pathway**: HSF1-mediated heat shock response  
    - **Cell line**: Neuro-2a or HEK cells with HSE-driven reporter  
    - **Readout**: Luminescence when proteotoxic stress activates HSF1  

11. **SR-MMP** (Mitochondrial Membrane Potential)  
    - **Assay format**: Dye uptake (e.g. TMRE or Mito-MPS) in hepatocytes  
    - **Readout**: Fluorescent ratio indicating mitochondrial depolarization  

12. **SR-p53** (p53 Response)  
    - **Pathway**: p53 tumor suppressor activation  
    - **Cell line**: HepG2 cells with p53-responsive luciferase reporter  
    - **Readout**: Luminescence when DNA damage or stress stabilizes p53  


### Model Summary
There are three models:
1. `GINE`: A simple edge-enhanced graph isomorphism network.
2. `GINEWithJK`: A more advanced edge-enhanced GIN that incorporates Jumping Knowledge to aggregate the embeddings from *all* hidden layers to influence the final prediction.
3. `GINEWithQNN`: An edge-enhanced GIN with the final readout/prediction ``nn.Linear`` layer replaced with a small quantum neural network.

### Results
`GINE`: $$0.702 \quad\text{AUC}$$       
`GINEWithJK`: $$0.716 \quad\text{AUC}$$          
`GINEWithQNN`: $$0.706 \quad\text{AUC}$$ (ideal simulator)

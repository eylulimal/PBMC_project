 # Work Plan - Day 1 to Day 14
 
 ## Day 1: Project Setup and Planning
 - **Task Division:** Eylul and Maria will jointly set up the repository, folder structure, and R environment.
 - **Result Exchange:** Code snippets and intermediate findings will be shared via Git/GitHub.
 - **File Responsibilities:** Shared responsibility , tracked via Git commits and "progress_log.md".
 - **Blockers & Communication:** Technical blockers will be discussed daily, documented in "progress_log.md".
 
 ## Day 2: Data Loading and Preprocessing 
 -**Tasks:**
 - Download and verify the H5 data files for all 4 donors.
 - Create Seurat objects for each donor ("donor1", "donor2", "donor3", "donor4").
 - Add metadata: donor, sex, age group.
 - Merge Seurat objects into a single combined object.
 - Document dataset source, donors, technology, and cell numbers in the Quarto report.
 - Add biological background section on PBMCs (T cells, B cells, NK cells, Monocytes, and DCs).
 - Create and orginize canonical marker gene list for major PBMC populations and export to "data/marker_genes_pbmc.csv"
 -**Deliverables:**
 - Merged Seurat object with metadata
 - Marker gene table CSV
 - Updated report draft and Git commit
 
  ## Day 3: Quality Control and Filtering
  -**Tasks:**
  - Calculate mitochondrial percentage ("percent.mt") for each cell.
  - Plot "nFeature_RNA", "nCount_RNA", and "percent.mt" per donor (violin and scatter plots).
  - Choose filtering tresholds ("nFeature_RNA > 200 & < 6000", "percent.mt < 15") and justify them.
  - Filter out low-quality cells, empty droplets, and potential doublets.
  - Add QC methods, plots, and filtering decisions to the Quarto report.
  -**Deliverables:**
  - QC violin and scatter plots
  - Filtered Seurat object
  - Written filtering decision in the report
  - Git commit and GitHub push
  
   ## Day 4: Normalization and Dimensionality Reduction
   -**Tasks:**
   - Apply global-scaling normalization ("LogNormalize") to the filtered count matrix.
   - Identify the top 2,000 highly variable features ("FindVariableFeatures").
   - Scale the data matrix to prepare for linear dimensionality reduction.
   - Run PCA ("RunPCA") and evaluate components.
   - Select the top 15 principal components ("dims = 1:15") for downstream analysis.
   - Run UMAP non-linear dimensionality reduction and visualize cells colored by donor.
   - Add normalization, PCA, UMAP methods, plots, and biological interpretations to the Quarto report.
   -**Deliverables:**
   - PCA plot and Elbow plot
   - UMAP plot colored by donor
   - Updated report draft with explanation
   - Git commit and GitHub push
   
    ## Day 5: Clustering
    -**Tasks**
    - Build nearest-neighbor graph using the top 15 principal components ("FindNeighbors").
    - Cluster cells at several resolutions (0.2, 0.4, 0.6, 0.8) using the Louvain algorithm ("FindClusters").
    - Compare clustering results visually using side-by-side UMAP plots.
    - Choose an optimal clustering resolution based on biological interpretability and cluster stability.
    - Explain the chosen resolution and justify the final parameters in the report.
    -**Deliverables:**
    - UMAP colored by cluster (showing final resolution groups)
    - Table/summary showing resolution values and corresponding number of clusters
    - One paragraph justifying the final resolution choice in the report draft
    - Updated report draft
    - Git commit and GitHub push
    
     ## Day 6: Marker Gene Analysis
     -**Tasks:**
     - Join data layers for Seurat v5 compatibility ("LoinLayers").
     - Find cluster marker genes across all distinct clusters ("FindAllMarkers").
     - Export top marker genes per cluster as a CSV file ("cluster_markers.csv").
     - Compare cluster markers with canonical PBMC markers (e.g., T cells, monocytes, B cells).
     - Add marker gene methods and first-pass interpretations to the Quarto report. 
     -**Deliverables:**
     - "cluster_markers.csv" file in the project directory.
     - Table of top 10 markers per cluster
     - First-pass cell type annotation table in the report
     - Updated report dreft
     - Git commit and GitHub push
     
      ## Day 7: Cell Type Annotation
      -**Tasks:**
      - Generate feature plots or dot plots for canonical markers.
      - Assign broad cell type labels based on marker gene expression.
      - Mark uncertain clusters clearly.
      - Create annotated UMAP map with cell type labels.
      - Explain annotation logic in the report.
      
      -**Deliverables:**
      -Marker dot plot
      - Annotated UMAP map with cell types
      - Final cell type annotation table
      - Updated report draft
      - Git commit
      
       ## Day 8: Donor Comparison
       -**Tasks:**
       - Count cells per annotated cell type and donor.
       - Calculate cell type proportions.
       - Make stacked bar plots to compare donors.
       - Interpret major donor differencess and variations.
       - Add donor comparison results and visualization to the Quarto report.
       
       -**Deliverables:**
       - Cell type count table
       - Cell type proportion table
       - Stacked bar plot by donor
       - Short interpretation
       - Updated report draft
       - Git commit
       
       ## Day 9: Exploratory Sex/Age Biology Layer
       -**Tasks:**
       - Research and define candidate gene sets for sex chromosome expression, X-linked immune relevance,
       and age-associated markers.
       - Create and export the candidate gene summary table as "sex_age_candidate_genes.csv".
       - Visualize gene expression patterns using Seurat "FeaturePlot".
       - Document biological interpretations and discuss dataset design limitations/confounding variables.
       
       -**Deliverables:**
       - "sex_age_candidate_genes.csv"
       - Sex chromosome and marker plots
       - Interpretation paragraph and reasoning 
       - Updated report draft
       - Git commit
       
        ## Day 10: Final Report and Presentation
        -**Tasks:**
        - Finalize the dynamic Quarto HTML report containing all clustering, UMAP, and marker gene identification 
        steps.
        - Optimize rendering peerformance and clean up code output using chunk caching and warning suppression rules.
        - Perform final repository synchronization and verify project deployment.
        
        -**Deliverables:**
        - Fully rendered "index.html" report
        - Updated GitHub repository and clean project documentation
        - Final project completion check
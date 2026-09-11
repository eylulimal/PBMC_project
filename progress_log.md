 ## 2026-08.24 (Day 1)
 
  ### Completed today
  - Created project folder structure ("data", "scripts", "results", "report").
  - Initialized Git repository and created ".gitignore".
  - Set up R environment with "renv" and installed required packages ("Seurat", "tidyverse", etc.).
  - Drafted initial project documentation ("README.md", "work_plan.md").
  
  ##  Problems encountered 
  - None so far.
  
  ### Decisions made
  - Decided to use "renv" to ensure package reproducibility across machines.
  
  ### Next steps
  - Download and verify the H5 data files for all 4 donors.
  
  ## 2026-08-25 (Day 2)
  
  ### Completed today
  - Documented dataset source, donors, technology, and cell numbers in the Quarto report.
  - Added a detailed biological background section on PBMCs (T cells, B cells, NK cells, Monocytes and DCs).
  - Created and orginized the canonical marker gene list for major PBMC populations.
  - Exported the marker gene table to "data/marker_genes_pbmc.csv" and updated the Quarto analysis report ("pbmc_analysis.qmd").
  
  ### Problems encountered
  
  - None so far.
  
  ### Decisions made
  - Structured the marker gened into an R list and converted it into a tidy data frame to facilitate downstream cell
  type annotation.
  
  ### Next steps
  - Proceed to Day 3 tasks:Quality Control (QC) and filtering of scRNA-seq data.
  
  ## 2026-08-26 (Day 3)
  
  ### Completed today
  - Created Seurat objects for all 4 donors and added metadata (donor, sex, age group).
  - Calculated mitochondrial gene percentage ("percent.mt") for each cell.
  - Generated and examined QC violin plots and scatter plots ("nFeature_RNA", "nCount_RNA", "percent.mt").
  - Defined and justified filtering tresholds ("nFeature_RNA > 200 & < 6000", "percent.mt < 15") yielding 21,586 high-quality cells.
  - Updated the Quarto analysis report with QC results and filtering decisions.
  
  ### Problems encountered
  - None so far.
  
 ### Decisions made 
 - Chlected specific tresholds based on standard PBMC quality control metrics to eliminate dying cells and doublets.
 
 ### Next steps
 - Proceed to Day 4: Normaliyation and Dimensionality Reduction.
 
 ## Day 4: Normalization & Dimensionality Reduction
 - Applied global-scaling normalization("LogNormalize", scale factor 10,000) and identified the top 2,000 highly variable features
 ("FindVariableFeatures").
 - Scaled the filtered gene expression matrix to prepare for linear dimensionality reduction.
 - Performed Principal Component Analysis ("RunPCA") and evaluated components to select the top 15 principal components
 ("dims = 1:15").
 - Generated UMAP non-linear dimensionality reduction and visualized the cells, confirming successful donor integration
 with minimal batch effects.
 - Updated the analysis report with new code blocks, UMAP visualizations, and interpretations.
 
  ### Problems encountered 
  - None so far.
  
  ### Decesions made
  - Selected the top 15 principal components for UMAP based on PCA evaluation to ensure robust biological clustering without
  capturing noise.
  
   ### Next steps
   - Proceed to Day 5 tasks: Clustering and marker gene identification.
   
  ## Day 5: Clustering
  - Built the K-nearest neighbor (KNN) graph using the top 15 principal components ("FindNeighbors").
  - Performed cell clustering across multiple resolutions (0.2, 0.4, 0.6, 0.8) using the Louvain algorithm 
  ("FindClusters").
  - Compared clustering outputs visually using UMAP side-by-side plots to evaluate cluster granularity.
  - Selected **resolution 0.4** as the optimal clustering parameter, yielding 17 distinct biological clusters while 
  avoiding over-clustering.
  - Visualized the final clustered cells on the UMAP reduction map with cluster labels.
  
   ###  Problems encountered
   - None so far.
   
   ### Decision made
   - Selected **resolution 0.4** as the optimal clustering parameter after systematically comparing multiple resolutions 
   (0.2, 0.4, 0.6, 0.8). This specific resolution provides an ideal balance by capturing major, well-established PBMC 
   populations (such as CD4+ T cells, CD8+ T cells, B cells, NK cells, and monocytes) while preventing artificial
   over-clustering and excessive subdivision of homogeneous cell groups, ensuring robust biological interpretability 
   for downstream marker gene analysis.
   
    ### Next steps
    - Proceed to Day 6: Marker Gene Analysis to identify signature genes for each cluster and assign cell type annotation.
   
    ## Day 6: Marker Gene Analysis
    -**Tasks:**
    - Joined data layers for Seurat v5 compatibility using "JoinLayers()".
    - Identified cluster marker genes across all 17 distinct clusters using "FindAllMarkers()" (Wilcoxon rank-sum
    test, "only.pos = TRUE", "min.pct = 0.25", "logfc.treshold = 0.25").
    - Exported the complete marker gene list as "cluster_markers.csv" into the project directory.
    - Filtered and inspected the top 10 marker genes for each cluster (e.g., *IL7R*, *CD3D* for T cells).
    
    -**Problems encountered:**
    - Addressed Seurat v5 layer seperation warning by successfully applying "JoinLayers()".
    - Accounted for longer computation time due to the large cell count (21,000 + cells) during differential expression
    testing.
    
    **Decision made:**
    - Used standard Wilcoxon testing with treshold parameters ("min.pct = 0.25", "logfc.treshold = 0.25") to ensure
    robust and statistically significant marker identification for all clusters without over-filtering.
    
    **Next steps:**
    - Proceed to Day 7: Cell Type Annotation based on canonical markers and export final annotated results.
    
     ## Day 7: Cell Type Annotation
     -**Tasks:**
     - Generated and inspected canonical marker expression using a DotPlot ("DotPlot") with known markers
     (*ILR7*, *CD3D*, *CD14*, *LYZ*, *MS4A1*, *NKG7*,etc.).
     - Assigned broad biological cell type labels to all clusters based on marker enrichment (e.g., CD4 T cells,
     CD14+ Monocytes, B cells, NK cells, Dendritic cells, Platelets).
     - Updated Seurat object identities using "RenameIdents()".
     - Visualized the final annotated cell populations on the UMAP reduction map with cluster labels ("DimPlot()").
     
     -**Problems encountered:**
     - None so far. All marker patterns matched well with canonical PBMC populations.
     
     **Decisions made:**
     - Successfully mapped numerical clusters to biological cell types, enabling clear downstream comparisons.
     
     **Next steps:**
     - Proceed to Day 8: Donor Comparison and differential proportion analysis. 
     
      ## Donor Comparison
      -**Tasks:**
      - Counted cells per annotated cell type and donor using cross-tabulation ("table()").
      - Calculated percentage proportions across samples ("prop.table()").
      - Created and visualized stacked bar plots ("ggplot2") to compare cell type abundances per donor.
      - Interpreted major donor differences and sample-specific variations.
      - Added donor comparison results and visualization to the main analysis report.
      
      -**Problems encountered:**
      - Encountered a minor aesthetics mapping error when calling "celltype" directly from metadata; resolved by 
      mapping active identities via "Idents(pbmc_filtered)".
      
      -**Decision made:**
      - Utilized proportional stacked bar charts ("position = "fill"") to normalize and fairly compare cell type
      frequencies across donors with varying total cell counts.
      
      -**Next steps:**
      - Proceed to Day 9: Exploratory Sex/Age Biology Layer and continue report documentation.
      
       ## Day 9: Exploratory Sex/Age Biology Layer
       -**Tasks:**
       - Researched and defined candidate gene sets covering sex chromosome expression ("XIST", "RPS4Y1"), X-linked immune
       relevance ("EIF1AX", "CD40LG"), and age-associated/inflammaging markers ("B2M", "S100A8").
       - Created and exported the candidate gene summary table as "sex_age_candidate_genes.csv".
       - Visualized gene expression patterns across cell populations using Seurat "FeaturePlot".
       - Documented biological interpretations, noting the limitations of confounding factors between donor identity,
       sex, and age in small cohorts.
       
       -**Problems encountered:**
       - None; candidate genes matched successfully with dataset row names.
       
       -**Decision made:**
       - Focused on exploratory marker expression and addressed the confounding structure of the dataset via structured 
       methodological reasoning rather than over-interpreting demographic effects.
       
       -**Next steps:**
       - Proceed to Day 10: Final Report and Presentation.
       
        ## Day 10: Final Report and Presentation
        -**Tasks:**
        - Finalized the Quarto ("index.qmd") HTML report incorporating all single-cell clustering, UMAP visualizations,
        and differential marker gene analysis.
        - Verified code optimization, caching parameters, and warning suppressions for a clean, reproducible output.
        - Prepared final project documentation, REAADME summary, and synchronized repository updates via Git.
        
        -**Problems encountered:**
        - None; technical rendering bottlenecks were successfully resolved using chunk-level caching and environment
        optimizations.
        
        -**Decision made:**
        - Completed project deployment and verified that the report accurately reflects all biological insights and 
        methodological workflow steps.
        
        -**Next steps:**
        - Project completed successfully; ready for final submission and presentation.
   
   
  
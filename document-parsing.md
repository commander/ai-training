Yes. Most research is about PDF parsing, because PDF stores layout and not structure. DOCX and Excel have less research, because they already contain structured XML. Below are the main sources.

## Surveys and comparisons (start here)

- **"Document Parsing Unveiled: Techniques, Challenges, and Prospects for Structured Information Extraction"** (Zhang et al., 2024, arXiv 2410.21169). This survey compares pipeline methods (layout detection, then OCR, then table and formula recognition) with end-to-end vision-language models.
- **"A Comparative Study of PDF Parsing Tools Across Diverse Document Categories"** (Adhikari & Agarwal, 2024, arXiv 2410.09871). This study tests tools such as PyMuPDF, pdfplumber, Nougat and others on different document types.
- **OmniDocBench** (Ouyang et al., 2024, arXiv 2412.07626). This is a benchmark to measure PDF parsing quality (text, tables, formulas, reading order).

## PDF parsing methods for training data

- **Nougat** (Meta, 2023, arXiv 2308.13418). A model that converts scientific PDFs to Markdown.
- **olmOCR** (Allen AI, 2025, arXiv 2502.18443). A vision-language model that extracts clean text from PDFs at large scale. Allen AI made it for LLM pretraining data. This paper is the closest match to your question.
- **MinerU** (Shanghai AI Lab, 2024, arXiv 2409.18839). An open-source pipeline for high-quality PDF extraction for LLM data.
- **Docling Technical Report** (IBM, 2024, arXiv 2408.09869). A toolkit that converts PDF, DOCX, PPTX and XLSX into one document format.
- **DocLayNet** (IBM, 2022, arXiv 2206.01062). A dataset for document layout analysis. Many parsers train on it.
- **Donut** (2022, arXiv 2111.15664). An OCR-free model that reads document images directly.

## Spreadsheets and tables

- **SpreadsheetLLM** (Microsoft, 2024, arXiv 2407.09025). This paper shows how to encode Excel sheets so that an LLM can use them. It covers cell compression and table structure.
- **"Table Meets LLM"** (Sui et al., 2024, WSDM). This paper compares table serialization formats (Markdown, HTML, JSON) for LLM input.

## Large-scale datasets that describe their extraction method

- **Dolma** (Allen AI, 2024, arXiv 2402.00159) and **peS2o**. These papers show how academic PDFs become pretraining text.
- **FinePDFs** (Hugging Face, 2025). A large PDF-based pretraining dataset. The documentation explains when they use text extraction and when they use OCR.

## Books

No book focuses only on document parsing for LLM datasets. These books have useful chapters about data pipelines:

- **LLM Engineer's Handbook** (Iusztin & Labonne, Packt, 2024). It covers data collection, cleaning and dataset creation for fine-tuning.
- **Hands-On Large Language Models** (Alammar & Grootendorst, O'Reilly, 2024). It covers text chunking and data preparation.
- **Build a Large Language Model (From Scratch)** (Raschka, Manning, 2024). It covers data preparation for pretraining and fine-tuning, but not parsing.

## Tools for DOCX and Excel

For these formats, tool documentation is more useful than papers. Microsoft **MarkItDown**, **Docling**, **Unstructured** and **Pandoc** convert DOCX and XLSX to Markdown or JSON.


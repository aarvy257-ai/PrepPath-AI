# Sample Opportunity Guidelines

This directory contains real-world scholarship and grant guideline documents used to test and validate the PrepPath AI extraction and evaluation pipeline.

### Files Included:
- `Guidelines_3042.pdf`: Official institutional scholarship guidelines used for eligibility parsing benchmarks.
- `PM-USP-CSSS.pdf`: Official guidelines for the Central Sector Scheme of Scholarship for College and University Students (PM-USP CSSS), providing complex multi-criteria criteria (income thresholds, board percentiles, domicile, quota categories) to benchmark deterministic rule evaluation.

These sample documents serve as test fixtures for the in-memory PyMuPDF extraction service (`app/services/pdf.py`) and Gemini structured output extraction (`app/ai/opportunity_analyzer.py`).

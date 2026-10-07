# Test Case 01 — Unknown CSV Export Bug

## Bug Report

Customer: Acme Analytics

Customer ID: CUST-1842

Ticket ID: ND-10318

Reported: 2026-10-07

Subject: CSV export freezes the dashboard

## Customer Message

Hi NovaDesk Support,

We're having an issue when exporting a large dataset from the Analytics dashboard.

When we select more than 10,000 rows and click "Export CSV", the dashboard becomes completely unresponsive. The browser tab stops responding for around 30 seconds and then the page eventually reloads.

We tested this with smaller exports and those work normally.

Steps we followed:

1. Open the Analytics dashboard.
2. Select a dataset containing more than 10,000 rows.
3. Click "Export CSV".
4. The dashboard becomes unresponsive.
5. After approximately 30 seconds, the page reloads.

Expected behavior:

The CSV should download successfully regardless of dataset size.

Actual behavior:

The dashboard freezes and the export does not complete.

Impact:

Our team uses CSV exports for weekly reporting, and the issue is preventing us from exporting our larger datasets.

This has happened consistently three times today.

Thanks,
Alex
Acme Analytics
# SETD1B promoter CRISPR protospacers

Reference: GRCh38/hg38, chr12:121,803,009–121,804,509, plus strand; length 1,501 bp. MANE Select TSS: chr12:121,804,009 (ENST00000604567.6 / NM_001353345.2), as specified. Sequence retrieved from the [Ensembl REST sequence/region endpoint](https://rest.ensembl.org/sequence/region/human/12:121803009..121804509:1?content-type=text/plain); uppercase sequence SHA-256: `e83cd5074df5a378787dbb9d8493131f5063f0513cb221d099abcd0d629a87d0`. Full-window GC fraction: **0.720853** (72.09%).

| Nuclease | Full window | Validation sub-window |
|---|---:|---:|
| SpCas9 | 443 | 135 |
| SaCas9 | 30 | 10 |
| Cas12f | 16 | 6 |
| Cas12a | 21 | 8 |

Validation sub-window: chr12:121,803,609–121,804,059 (451 bp), scanned independently with both the full protospacer and PAM required within that sub-window. GC fraction: **0.716186** (71.62%). Expected approximate counts: SpCas9 ~135, SaCas9 ~10, Cas12f ~6, GC ~71.6%.

## Coordinates and fields

- `guides.tsv`: genomic start and end coordinates are **1-based inclusive**. `g_start` and `g_end` cover only the protospacer; `pam_start` and `pam_end` cover only the PAM. `strand` is the genomic strand on which the displayed protospacer and PAM sequences are written 5′ to 3′.
- `guides.bed`: BED6 coordinates are **0-based, half-open**, for the protospacer only: `start = g_start - 1`, `end = g_end`. Name is `guide_id`, score is 0, and strand matches the TSV. Rows correspond one-to-one to TSV rows.
- `pos_vs_tss` = `(g_start + g_end) / 2 - 121804009` in genomic bases. Negative means upstream of the plus-strand TSS; even-length protospacers have half-integer midpoints. The value is not rounded to an integer.
- `pam_side` is 5 or 3, relative to the protospacer on the displayed strand. `gc_frac` is the protospacer GC fraction rounded to nine decimal places. `has_polyT` flags `TTTT`; `gc_extreme` flags GC fraction strictly below 0.20 or above 0.80. Neither flag filters guides.
- Overlapping PAMs are included. Both PAM and complete protospacer must lie within the requested window. Guides are not deduplicated across nucleases. IDs are stable for the same reference and scan order: nuclease, plus strand then minus strand, ascending PAM position in each strand's 5′→3′ sequence. IDs in a separately scanned sub-window are independent.

## Reproduce

Run `python3 enumerate_guides.py` in a network-enabled environment. The script fetches the reference from Ensembl, asserts its length is 1,501 bases, independently scans the 451 bp sub-window, and rewrites the TSV, BED, and this README. Standard Python 3 library only.

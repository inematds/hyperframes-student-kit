# FALHAS

| data | o que quebrou | menor correção | prompt \| infra |
| --- | --- | --- | --- |
| 2026-09-13 | flux2-klein com `-n 3 --seed 11` gerou 3 imagens idênticas (seed fixo repete a variação) | omitir `--seed` ou passar seeds distintos por variação; conferir md5 antes de usar | prompt |

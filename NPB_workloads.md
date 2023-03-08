# NAS Parallel Benchmarks Callgraphs

## bt.x - brief version

`NPB3.3.1/NPB3.3-MPI/sys/setparams.c`

| class |      problem_size | niters |
|-------|-------------------|--------|
|     S |          12x12x12 |     60 |
|     W |          24x24x24 |    200 |
|     A |          64x64x64 |    200 |
|     B |        102x102x102|    200 |
|     C |        162x162x162|    200 |
|     D |        408x408x408|    250 |
|     E |     1020x1020x1020|    250 |

```
notations:
  . (i,j,k,m) \in NxNxNx5
  . forall orders are not reserved
main()
  |__ call initialize
      |__ forall (i,j,k): call exact_solution x 6
      |__ forall (i,j): call exact_solution x 2
      |__ forall (i,k): call exact_solution x 2
      |__ forall (j,k): call exact_solution x 2
  |__ call exact_rhs
      |__ forall (i,j,k): call exact_solution x 2
  |__ call adi
  |__ call initialize
  |__ # ROI BEGIN
  |__ call adi x200
      |__ call compute_rhs: O(N^3)
      |__ call x_solve, y_solve, z_solve
          |__ call lhsinit: O(1)
          |__ call binvcrhs x 2 : O(1)
          |__ call matvec_sub x 2: O(1)
          |__ call matmul_sub x 2: O(1)
          |__ call binvrhs: O(1)
      |__ call add: O(N^3)
  |__ # ROI END
```

# inference-systems-lab

Working repository for an experimental LLM inference systems lab:

kernels → compiler/runtime integration → serving → distributed systems → production

## Layout

### Systems path
| Path | Contents |
|---|---|
| `kernels/` | CUDA/Triton/HIP implementations + design notes |
| `compiler/` | Custom ops, IR inspection, `torch.compile` integration |
| `serving/` | Engine deployments, configs, bottleneck maps |
| `ops/` | Runbooks, dashboards, release/regression gates |

### Evaluation and experimentation
| Path | Contents |
|---|---|
| `verifier/` | Correctness infrastructure |
| `verifier/oracles/` | Trusted reference implementations |
| `verifier/tests_public/` | Correctness tests visible to implementation/agent workflows |
| `verifier/heldout/` | Held-out-test policy and released evaluation sets; active held-out tests remain external |
| `bench/` | Benchmark harnesses (experiment-contract compliant) |
| `profiling/` | Traces, counter dumps, rooflines |
| `experiments/` | Hypothesis-first experiment records |
| `reports/` | Technical write-ups synthesizing results and lessons from experiments |
| `agent-runs/` | Full provenance log for every AI-assisted coding loop |


## Conventions

- A design note defining the semantic/numerical contract and bottleneck hypothesis is committed before the corresponding implementation
- Kernel directories are self-contained: source, local headers, bindings, and build/test entry points live together
- Python bindings resolve native libraries relative to the binding module (`Path(__file__).resolve().parent`) never relative to the caller's working directory or via machine-specific literal paths
- Correctness is established against trusted references before performance claims are accepted
- No benchmark numbers are reported unless the run satisfies the experiment contract
- Experiments preserve the environment, exact commands, raw outputs, and interpretation needed to reproduce the result
- Every AI-assisted implementation loop is recorded in `agent-runs/`

## Human / agent roles

The default workflow is human-first: the primary goal of this repository is to learn the systems work directly, while still reflecting the reality of modern human-agent engineering workflows. Understanding when the automation is wrong, why it's wrong, what it should search next, and whether its result belongs in a production inference system.

- Each kernel is designed and implemented by hand before any agent-generated implementation is evaluated
- The human implementation establishes the initial correctness and performance baseline
- The human version is committed before the corresponding agent run begins
- The agent is then given the same design contract, API constraints, and public tests used for the human implementation
- The agent is not shown the human implementation
- Active held-out tests live outside the agent-accessible workspace and are fetched only by the evaluation job
- After an experiment is frozen, its held-out tests may be released under `verifier/heldout/released/` for reproducibility; released tests are not reused for future hidden evaluation
- Human and agent implementations are evaluated with the same correctness checks, benchmark methodology, hardware, and profiling procedure
- Agent runs, prompts, generated candidates, and accepted modifications are recorded under `agent-runs/`
- Performance conclusions are based on measured results and not on agent-generated estimates
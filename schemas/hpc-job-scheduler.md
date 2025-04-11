# HPC Job Scheduler Event Schema

NOTE: Work in Progress!

Tools being targeted:

- Slurm Job Logs
- Flux Job Logs

## Job Completion Log Schema

This schema is for the job completion information of a completed job. In Slurm, this information is controlled by the `JobComp*` fields, such as `JobCompType` and `JobCompLoc` (see: <https://slurm.schedmd.com/slurm.conf.html> for more information).

The columns for "Slurm" and "Flux" represent the field in that data set which is used to retrieve the value.

| **Field Name**           | **Data Type** | **Field Explanation**                                                                                                                                         | **Slurm**       | **Flux**         | **LSF**           |
| ------------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------- | ---------------- | ----------------- |
| event.dataset\*          | keyword       | Name of the dataset "slurm.joblog"                                                                                                                            | "slurm.joblog"  | "flux.joblog"    | "lsf.joblog"      |
| job.id                   | keyword       | Scheduler managed identifier of the job                                                                                                                       | JobId           | id               | LSF.jobId         |
| user.name\*              | keyword       | User name of who submitted the job                                                                                                                            | UserId          | username         | LSF.userName      |
| user.id\*                | keyword       | User ID number of who submitted the job                                                                                                                       | UserId          | userId           | LSF.userId        |
| group.id\*               | keyword       | Group ID number of who submitted the job                                                                                                                      | GroupId         |                  | LSF.              |
| group.name\*             | keyword       | Group name of who submitted the job                                                                                                                           | GroupId         |                  | LSF.              |
| job.name                 | keyword       | The name of the job, input by the user                                                                                                                        | Name            | jobspec.name     | LSF.jobName       |
| event.outcome\*          | keyword       | The final state of the job                                                                                                                                    | JobState        | result           | LSF.              |
| job.queue                | keyword       | The queue the job was submitted to                                                                                                                            | Partition       | queue            | LSF.queue         |
| job.timelimit            | keyword       | Maximum time job could have ran for.                                                                                                                          | TimeLimit       | expiration       | LSF.              |
| job.timelimit_seconds    | long          | Maximum time job could have ran for, converted to seconds.                                                                                                    |                 |                  | LSF.              |
| event.start\*            | date          | Time that the job started.                                                                                                                                    | StartTime       | t_run            | LSF.startTime     |
| event.end\*              | date          | Time that the job ended.                                                                                                                                      | EndTime         | t_inactive       | LSF.              |
| event.duration\*         | long          | The actual amount of time the job ran for (event.start - event.end) (in nanoseconds)                                                                          |                 | jobspec.duration | LSF.              |
| event.duration_seconds\* | long          | The actual amount of time the job ran for (event.start - event.end) (in seconds)\*\*                                                                          |                 |                  | LSF.              |
| job.node.list            | text          | list of nodes being used, can be a range ex: corona[155, 157, 180-190]                                                                                        | NodeList        | R.hostlist       | LSF.execHosts     |
| job.node.count           | integer       | Count of nodes used                                                                                                                                           | NodeCnt         | nnodes           | LSF.numProcessors |
| job.proc.count           | integer       | Number of processors (typically number of processes per node \* number of nodes)                                                                              | ProcCnt         |                  | LSF.              |
| job.cwd                  | text          | Current working directory when job submitted                                                                                                                  | WorkDir         |                  | LSF.cwd           |
| job.reservation          | keyword       |                                                                                                                                                               | ReservationName |                  | LSF.              |
|                          |               |                                                                                                                                                               | Tres            |                  | LSF.              |
| job.bank                 | keyword       | Job bank used during submission                                                                                                                               | Account         | bank             | LSF.              |
| job.urgency              | keyword       |                                                                                                                                                               | QOS             | urgency          | LSF.              |
| job.project              | keyword       | The name of the project the job is associated with                                                                                                            | WcKey           |                  | LSF.              |
| host.cluster             | keyword       | The cluster the job ran on.                                                                                                                                   | Cluster         |                  | LSF.              |
| job.submittime           | date          | Time the job was submitted.                                                                                                                                   | SubmitTime      | jobspec.t_submit | LSF.submitTime    |
| job.eligibletime         | date          |                                                                                                                                                               | EligibleTime    |                  | LSF.              |
| job.exit_code            | integer       |                                                                                                                                                               | ExitCode        | waitstatus       | LSF.exitStatus    |
| job.exit_signal          | integer       | The signal number, if the job's termination was caused by a signal being sent.                                                                                | ExitCode        |                  | LSF.              |
| job.queue_time \*\*      | integer       | The time in seconds that the job was waiting in the queue (`end time - eligible time`) - TODO: This doesn't seem like the right equation (`start - submit` ?) |                 |                  | LSF.              |
| job.scheduler            | keyword       | The job scheduler used                                                                                                                                        | "slurm"         | "flux"           | "lsf"             |
| job.exception_type       | keyword       | The type of exception that was raised on a job.                                                                                                               |                 | exception_type   | LSF.              |
| job.exception_note       | text          | Any notes or additional details containing the reason for a job exception.                                                                                    |                 | exception_note   | LSF.              |
| message\*                | text          | The entire original event. Keep for debugging purposes                                                                                                        |                 |                  | LSF.              |

- Data Types are as defined by [Elastic Field Data Types](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)
- \* Event fields defined by the [Elastic Common Schema (ECS)](https://www.elastic.co/guide/en/ecs/current/ecs-field-reference.html)
- \*\* Indicates that value is derived from other values, and not reported directly from the tool.

Fields in LSF that don't pair with Slurm / Flux:

- `LSF.version`: Version of LSF
- `LSF.options`
- `LSF.beginTime`
- `LSF.termTime`
- `LSF.resReq`: `\"1*{select[LN]span[hosts=1]} + 200*{select[CN]span[ptile=40]}\" `
- `LSF.dependCond`
- `LSF.preExecCmd`
- `LSF.fromHost`: Host from which the job was submitted
- `LSF.inFile`
- `LSF.outFile`: `\"bilinear-final-mp_max-optimism_batchsize_400_numrounds_16_div_True_r19.txt\" `
- `LSF.errFile`

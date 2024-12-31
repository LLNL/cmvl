# HPC Job Scheduler Event Schema

NOTE: Work in Progress!

Tools being targeted:

- Slurm Job Logs
- Flux Job Logs

## Job Completion Log Schema

This schema is for the job completion information of a completed job. In Slurm, this information is controlled by the `JobComp*` fields, such as `JobCompType` and `JobCompLoc` (see: <https://slurm.schedmd.com/slurm.conf.html> for more information).

The columns for "Slurm" and "Flux" represent the field in that data set which is used to retrieve the value.

| **Field Name**           | **Data Type** | **Field Explanation**                                                                | **Slurm**       | **Flux**         |
| ------------------------ | ------------- | ------------------------------------------------------------------------------------ | --------------- | ---------------- |
| event.dataset\*          | keyword       | Name of the dataset "slurm.joblog"                                                   | "slurm.joblog"  | "flux.joblog"    |
| job.id                   | keyword       | Scheduler managed identifier of the job                                              | JobId           | id               |
| user.name\*              | keyword       | User name of who submitted the job                                                   | UserId          | username         |
| user.id\*                | keyword       | User ID number of who submitted the job                                              | UserId          | userId           |
| group.id\*               | keyword       | Group ID number of who submitted the job                                             | GroupId         |                  |
| group.name\*             | keyword       | Group ID number of who submitted the job                                             | GroupId         |                  |
| job.name                 | keyword       | The name of the job, input by the user                                               | Name            | jobspec.name     |
| event.outcome\*          | keyword       | The final state of the job                                                           | JobState        | result           |
| job.queue                | keyword       | The queue the job was submitted to                                                   | Partition       | queue            |
| job.timelimit            | keyword       | Maximum time job could have ran for.                                                 | TimeLimit       | expiration       |
| job.timelimit_seconds    | long          | Maximum time job could have ran for, converted to seconds.                           |                 |                  |
| event.start\*            | date          | Time that the job started.                                                           | StartTime       | t_run            |
| event.end\*              | date          | Time that the job ended.                                                             | EndTime         | t_inactive       |
| event.duration\*         | long          | The actual amount of time the job ran for (event.start - event.end) (in nanoseconds) |                 | jobspec.duration |
| event.duration_seconds\* | long          | The actual amount of time the job ran for (event.start - event.end) (in seconds)\*\* |                 |                  |
| job.node.list            | text          | list of nodes being used, can be a range ex: corona[155, 157, 180-190]               | NodeList        | R.hostlist       |
| job.node.count           | integer       | Count of nodes used                                                                  | NodeCnt         | nnodes           |
| job.proc.count           | integer       | Number of processors (typically number of processes per node \* number of nodes)     | ProcCnt         |                  |
| job.cwd                  | text          | Current working directory when job submitted                                         | WorkDir         |                  |
| job.reservation          | keyword       |                                                                                      | ReservationName |                  |
|                          |               |                                                                                      | Tres            |                  |
| job.bank                 | keyword       | Job bank used during submission                                                      | Account         | bank             |
| job.urgency              | keyword       |                                                                                      | QOS             | urgency          |
| job.project              | keyword       | The name of the project the job is associated with                                   | WcKey           |                  |
| host.cluster             | keyword       | The cluster the job ran on.                                                          | Cluster         |                  |
| job.submittime           | date          | Time the job was submitted.                                                          | SubmitTime      | jobspec.t_submit |
| job.eligibletime         | date          |                                                                                      | EligibleTime    |                  |
| job.exit_code            | integer       |                                                                                      | ExitCode        |                  |
| job.exit_signal          | integer       | The signal number, if the job's termination was caused by a signal being sent.       | ExitCode        |                  |
| job.scheduler            | keyword       | The job scheduler used                                                               | "slurm"         | "flux"           |
| event.original\*         | text          | The entire original event. Keep for debugging purposes                               |                 |                  |

- Data Types are as defined by [Elastic Field Data Types](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)
- \* Event fields defined by the [Elastic Common Schema (ECS)](https://www.elastic.co/guide/en/ecs/current/ecs-field-reference.html)
- \*\* Indicates that value is derived from other values, and not reported directly from the tool.

# HPC Job Scheduler Event Schema

NOTE: Work in Progress!

Tools being targeted:

- Slurm Job Logs
- Flux Job Logs

## Job Log Schema

| **Field Name**         | **Field Explanation**                                                                | **Slurm**       | **Flux**         |
| ---------------------- | ------------------------------------------------------------------------------------ | --------------- | ---------------- |
| job.id                 | Unique identifier of the job                                                         | JobId           | id               |
| user.name              | User name of who submitted the job                                                   | UserId          | username         |
| user.id                | User ID number of who submitted the job                                              | UserId          | userId           |
| group.id               | Group ID number of who submitted the job                                             | GroupId         |                  |
| group.name             | Group ID number of who submitted the job                                             | GroupId         | jobspec.name     |
| event.outcome          | The final state of the job                                                           | JobState        | result           |
| job.queue              | The queue the job was submitted to                                                   | Partition       | queue            |
| job.timelimit          | Maximum time job could have ran for.                                                 | TimeLimit       | expiration       |
| event.start            | Time that the job started.                                                           | StartTime       | t_run            |
| event.end              | Time that the job ended.                                                             | EndTime         | t_inactive       |
| event.duration         | The actual amount of time the job ran for (event.start - event.end) (in nanoseconds) |                 | jobspec.duration |
| event.duration_seconds | The actual amount of time the job ran for (event.start - event.end) (in seconds)     |                 |                  |
| job.node.list          | list of nodes being used, can be a range ex: corona[155, 157, 180-190]               | NodeList        | R.hostlist       |
| job.node.count         | Count of nodes used                                                                  | NodeCnt         | nnodes           |
| job.proc.count         | Number of processors (typically number of processes per node \* number of nodes)     | ProcCnt         |                  |
| job.cwd                | Current working directory when job submitted                                         | WorkDir         |                  |
| job.reservation        |                                                                                      | ReservationName |                  |
|                        |                                                                                      | Tres            |                  |
| job.bank               | Job bank used during submission                                                      | Account         | bank             |
| job.urgency            |                                                                                      | QOS             | urgency          |
| job.project            | The name of the project the job is associated with                                   | WcKey           |                  |
| host.cluster           | The cluster the job ran on.                                                          | Cluster         |                  |
| job.submittime         | Time the job was submitted.                                                          | SubmitTime      | jobspec.t_submit |
| job.eligibletime       |                                                                                      | EligibleTime    |                  |
| job.exit_code          |                                                                                      | ExitCode        |                  |
| job.exit_signal        | The signal number, if the job's termination was caused by a signal being sent.       | ExitCode        |                  |
| job.scheduler          | The job scheduler used                                                               | "slurm"         | "flux"           |
| event.original         | The entire original event. Keep for debugging purposes                               |                 |                  |

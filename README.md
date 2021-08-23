
```json
when i design a simulation with the following plan: {
request-exec-plan: {
    executions: [
            {
                request: http: //resim.com/MySimulationPlan-SubPlan-A
                sendAfter: 5s
                responseDelay: 0s
                responseOutCome: succeed //(outcomes can be *Succeed (2xx), *Fail (5xx), SucceedAfterNTmes: (Fail until N times requested, then Succeed)
            },
            {
                request: http: //resim.com/MySimulationPlan-SubPlan-A
                sendAfter: 15s
                responseDelay: 20s
                responseOutCome: fail,
            },
            {
                request: http: //resim.com/MySimulationPlan-SubPlan-A
                sendAfter: 30s
                responseDelay: 5s
                responseOutCome: succeed,
            }
        ]
    }, 
resiliency-plan: {
        strategy: {
        circuitBreaker: {
            breakAfterNFailures: 5,
            breakDuration: 1m
            }
        retry: {
            maxRetries: 3,
            }
        timeout: {
            timeoutAfter: 2s
            }
        }
    }
}

WorstCaseSimExecTime = T + (r w/ longest: rSendafter + min(rDelay, tO ?? rDelay)) + (rT ?? 1 * (r w/ longest: rSendafter + min(rDelay, tO ?? rDelay)))
MaxAllowedSimulationExecutionDuration = 1minute
simulationduration = min(totalSimExecTime, MaxAllowedSimulationExecutionDuration)
```
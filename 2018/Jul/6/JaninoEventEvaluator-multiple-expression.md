#### EvaluatorFilter

[`EvaluatorFilter`](https://logback.qos.ch/xref/ch/qos/logback/core/filter/EvaluatorFilter.html) is a generic filter encapsulating an `EventEvaluator`. As the name suggests, an [`EventEvaluator`](https://logback.qos.ch/xref/ch/qos/logback/core/boolex/EventEvaluator.html) evaluates whether a given criteria is met for a given event. On match and on mismatch, the hosting `EvaluatorFilter` will return the value specified by the onMatch or onMismatch properties respectively.

**JaninoEventEvaluator**

Logback-classic ships with another concrete `EventEvaluator` implementation called [JaninoEventEvaluator](https://logback.qos.ch/xref/ch/qos/logback/classic/boolex/JaninoEventEvaluator.html) taking an arbitrary Java language block returning a boolean value as the evaluation criteria. We refer to such Java language boolean expressions as "*evaluation expressions*". Evaluation expressions enable great flexibility in event filtering. 



Here is a concrete example.

```xml
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <filter class="ch.qos.logback.core.filter.EvaluatorFilter">      
      <evaluator> <!-- defaults to type ch.qos.logback.classic.boolex.JaninoEventEvaluator -->
        <expression>return message.contains("billing");</expression>
      </evaluator>
      <OnMismatch>NEUTRAL</OnMismatch>
      <OnMatch>DENY</OnMatch>
    </filter>
    <encoder>
      <pattern>
        %-4relative [%thread] %-5level %logger - %msg%n
      </pattern>
    </encoder>
  </appender>

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

Notice: Once there are more than one <expression> in the <evaluator> section, the evaluator will evaluate them respectively. If all the expression gives the 'true' result, the evaluator will take this as 'match'. Otherwise, it will take it as 'mismatch'.



So if we want to filter log events which meets of the criteria, just set one expresstion with OR operator. 

Like this: 

```xml
<configuration>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <filter class="ch.qos.logback.core.filter.EvaluatorFilter">      
      <evaluator> <!-- defaults to type ch.qos.logback.classic.boolex.JaninoEventEvaluator -->
        <expression>return message.contains("billing") || logger.contains("network");</expression>
      </evaluator>
      <OnMismatch>NEUTRAL</OnMismatch>
      <OnMatch>DENY</OnMatch>
    </filter>
    <encoder>
      <pattern>
        %-4relative [%thread] %-5level %logger - %msg%n
      </pattern>
    </encoder>
  </appender>

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```


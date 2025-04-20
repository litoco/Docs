# PromQL examples

1. Filter data on date and time:
Suppose we wanted to find how many 200 requests were processed between 8:00 AM and 8:10 AM UTC we can use the following [promql query](https://demo.promlabs.com/query?g0.expr=prometheus_http_requests_total%7Bstatus%3D%22200%22%7D+%0Aand+on%28%29+day_of_week%28%29+%3D%3D+0+%0Aand+on%28%29+hour%28%29+%3D%3D+8+%0Aand+on%28%29+minute%28%29+%3E%3D+0+%0Aand+on%28%29+minute%28%29+%3C%3D+10&g0.show_tree=1&g0.tab=table&g0.range_input=10d&g0.res_type=auto&g0.res_density=medium&g0.display_mode=lines&g0.show_exemplars=0):
    ```
    prometheus_http_requests_total{status="200"} # successful requests
    and on() day_of_week() == 0 # on sunday, week starts with sunday so sunday means 0
    and on() hour() == 8 # UTC hour timing, promql stores data in UTC
    and on() minute() >= 0 # start time
    and on() minute() <= 10 # end time
    ```

2. Convert data into percentage:
Suppose we want to get CPU utilization percentage, we can use the following promql query:
    ```
    TBD
    ```

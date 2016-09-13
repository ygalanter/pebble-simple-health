<b>pebble-simple-health</b> is a simple library that provides basic health info for user of Pebble smartwatch.

<i>This is a Beta version with Diorite support, build with SDK4</i>

Available information:

 - Step count
 - Distance walked
 - Sleep time
 - Deep Sleep Time
 - Time Active
 - Calories burned at rest
 - Calories burned while active

Every indicator has respetive "goal" value, calculated as an average from past days.

To use the package install it into your project and include `pebble-simple-health/pebble-simple-health.h` into code file. Note that pebble-simple-health depends on pebble-events package.

##Availale functions:

`bool health_is_available()` - returns `true` if health services are available, otherwise returns `false`. Available after call to `health_init`

`void health_init(health_callback *callback_proc)` - initializes health services, parameter is a callback procedure that will be called when health data is updated

`int health_get_metric_sum(HealthMetric metric)` -  gets current daily value for [requested metrics](https://developer.pebble.com/docs/c/Foundation/Event_Service/HealthService/#HealthMetric)

`int health_get_metric_goal(HealthMetric metric)` - gets goal value for requested metric.

`void health_deinit();` - deninitializes health service

##Example usage

```
void health_updated() {
  APP_LOG(APP_LOG_LEVEL_INFO, "Steps taken: %d", health_get_metric_sum(HealthMetricStepCount));
}

void handle_init(){
  health_init(health_updated);
  
  if (!health_is_available()) {
    APP_LOG(APP_LOG_LEVEL_DEBUG, "Health service is not available");
  }
}

void handle_deinit(){
  health_deinit();
}


int main(void) {
  handle_init();
  app_event_loop();
  handle_deinit();
}


```


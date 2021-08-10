# Retask
NuGet package for scheduling jobs in .NETCore 3.1 apps

## Install

## Use
Write at least one class IJob impl and one IScheduleSettings impl:
```CSharp
namespace HelloWorld {
    public class HelloJob : IJob {
	//process last run DateTime to calculate next run DateTime
	//there job will be executed every second
        public DateTime NextRun(DateTime last) => (last == default ? DateTime.Now : last).AddSeconds(1);

	//this method will be executed in Scheduler asynchronously
	public async Task Do() {
            Console.WriteLine("Hello world! Im wanna sleep :(");
	    await Task.Delay(1000 * 5); //sleep 5 seconds
	    Console.WriteLine("Nice dreams!");
	}
    }

    //IScheduleSettings provides default setting, but you can override the necessary ones
    public class MySettings : IScheduleSettings {}
}
```
Ok, now describe your IServiceCollection IJob and IScheduleSettings impls and Schedule IHostedService:
```CSharp
 	... some code here ...
	public void ConfigureServices(IServiceCollection services) {
    	    services.AddTransient<IJob, HelloJob()
		    .AddTransient<IScheduleSettings, MySettings>()
                    .AddHostedService<Scheduler>();
	}
	... some code here ...
```
Mission success! Your job will be automatically posted into Scheduler and executed with NextRun rule.

## Contribute

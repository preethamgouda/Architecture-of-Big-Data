# Architecture of Big Data System

## MapReduce Assignment

Submitted By- Preetham Gouda

**Find the count of each word from a given input file.

**Count the number of words based of character length in a given file

**Create dataset with fields like ‘Student Name’, ‘Institute’, ‘Program Name’, and ‘Gender’ and solve following questions.**

1. Compute number of students from each Institute.
2. Number of students enrolled to any program.
3. Number of ‘boy’ and ‘girl’ students.
4. Number of ‘boy’ and ‘girl’ students from selected Institute.

**Dataset: Temperature of Indian Cities. Fields of dataset are Date, Average Temperature, City, Country, Latitude and Longitude (Dataset is attached). Solve following questions**

1. Find maximum and minimum temperature of all cities from the given dataset
2. Count number of data point for each city.
3. Find the maximum and minimum temperature for city Bangalore from the given dataset.
4. Find the maximum and minimum temperature for any given city from the given dataset. City name should be passed through command line argument.


###Total Character Count Count


```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.Reducer.Context;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;


public class characterCount {
	public static class testCharacterCountMapper extends Mapper<LongWritable, Text,IntWritable,IntWritable> {
		public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
					String line=value.toString();
					StringTokenizer token=new StringTokenizer(line);
					while(token.hasMoreTokens()) {
						String str=token.nextToken();
						value.set(str);
						context.write(new IntWritable(str.length()), new IntWritable(1));
					}
					
					
					}
					
		}
	
	public static class testCharacterCountReducer extends Reducer <IntWritable,IntWritable,IntWritable,IntWritable > {
		public void reduce(IntWritable key,Iterable <IntWritable> value,Context context) 
			throws IOException, InterruptedException {
			int sum=0;
		for(IntWritable x:value) {
			sum++;
		}
				context.write(key, new IntWritable(sum));
		}
	}
	
	public static void main(String[] args) throws Exception{
		// TODO Auto-generated method stub
		Configuration conf = new Configuration();
		
		Job job = Job.getInstance(conf, "character count");

		job.setJarByClass(characterCount.class);
		job.setMapperClass(testCharacterCountMapper.class);
		job.setReducerClass(testCharacterCountReducer.class);

		job.setOutputKeyClass(IntWritable.class);
		job.setOutputValueClass(IntWritable.class);

		FileInputFormat.addInputPath(job, new Path(args[0]));
		
		FileOutputFormat.setOutputPath(job, new Path(args[1]));

		System.exit(job.waitForCompletion(true) ? 0: 1);
	}
}
```

####Word Count

```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.Reducer.Context;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;


public class characterCount {
	public static class testCharacterCountMapper extends Mapper<LongWritable, Text,IntWritable,IntWritable> {
		public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
					String line=value.toString();
					StringTokenizer token=new StringTokenizer(line);
					while(token.hasMoreTokens()) {
						String str=token.nextToken();
						value.set(str);
						context.write(new IntWritable(str.length()), new IntWritable(1));
					}
					
					
					}
					
		}
	
	public static class testCharacterCountReducer extends Reducer <IntWritable,IntWritable,IntWritable,IntWritable > {
		public void reduce(IntWritable key,Iterable <IntWritable> value,Context context) 
			throws IOException, InterruptedException {
			int sum=0;
		for(IntWritable x:value) {
			sum++;
		}
				context.write(key, new IntWritable(sum));
		}
	}
	
	public static void main(String[] args) throws Exception{
		// TODO Auto-generated method stub
		Configuration conf = new Configuration();
		
		Job job = Job.getInstance(conf, "character count");

		job.setJarByClass(characterCount.class);
		job.setMapperClass(testCharacterCountMapper.class);
		job.setReducerClass(testCharacterCountReducer.class);

		job.setOutputKeyClass(IntWritable.class);
		job.setOutputValueClass(IntWritable.class);

		FileInputFormat.addInputPath(job, new Path(args[0]));
		
		FileOutputFormat.setOutputPath(job, new Path(args[1]));

		System.exit(job.waitForCompletion(true) ? 0: 1);
	}
}
```

##### Create dataset with fields like ‘Student Name’, ‘Institute’, ‘Program Name’, and ‘Gender’ and solve following questions.

1. Compute number of students from each Institute.
2. Number of students enrolled to any program.
3. Number of ‘boy’ and ‘girl’ students.
4. Number of ‘boy’ and ‘girl’ students from selected Institute.

```java
import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class Student {
    
    public static class testMap_institute extends Mapper<LongWritable,Text, Text, IntWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
            String line = values.toString();
            String[] insti = line.split(" ");
            //String insti = line.substring(0,3);
            String par = context.getConfiguration().get("INSTITUTE");
            if(insti[1].equals(par))
            {
            context.write(values,new IntWritable(1));
            }
    }}
  
    public static class testMap_program extends Mapper<LongWritable,Text, Text, IntWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
            String line = values.toString();
            String[] program = line.split(" ");
            String par = context.getConfiguration().get("PROGRAM");
            if(program[2].equals(par)){
            context.write(new Text(program[2]),new IntWritable(1));
            }
    }}
   
    public static class testMap_gender extends Mapper<LongWritable,Text, Text, IntWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
            String line = values.toString();
            String[] gender = line.split(" ");
            String par = context.getConfiguration().get("GENDER");
            if(gender[3].equals(par)){
            context.write(new Text(gender[3]),new IntWritable(1));
            }
    }}
    public static class testMap_instigender extends Mapper<LongWritable,Text, Text, IntWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
            String line = values.toString();
            String[] insti = line.split(" ");
            String par = context.getConfiguration().get("INSTITUTE");
            if(insti[1].equals(par)){
            context.write(new Text(insti[3]),new IntWritable(1));
            }
    }}
    public static class testReduce extends Reducer<Text,IntWritable,Text,IntWritable> {
        public void reduce(Text key, Iterable<IntWritable> values,Context context) throws IOException, InterruptedException{
        	int totalCount = 0;
			for(IntWritable x: values) {
				totalCount += x.get();
			}
			context.write(key, new IntWritable(totalCount));
        }
        
    }
    public static void main(String[] args) throws Exception {
        // TODO Auto-generated method stub
        Configuration conf = new Configuration();
        conf.set("INSTITUTE", args[5]);
        Job job = Job.getInstance(conf,"student_institute");
        job.setJarByClass(studentDetails2.class);
        job.setMapperClass(testMap_institute.class);
        job.setReducerClass(testReduce.class);
        
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        
        FileInputFormat.addInputPath(job,new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        
        job.waitForCompletion(true);
        
        Configuration conf1 = new Configuration();
        conf1.set("PROGRAM", args[5]);
        Job job1 = Job.getInstance(conf1,"student_program");
        job1.setJarByClass(studentDetails2.class);
        job1.setMapperClass(testMap_program.class);
        job1.setReducerClass(testReduce.class);
        
        job1.setOutputKeyClass(Text.class);
        job1.setOutputValueClass(IntWritable.class);
        
        FileInputFormat.addInputPath(job1,new Path(args[0]));
        FileOutputFormat.setOutputPath(job1, new Path(args[2]));
        
        job1.waitForCompletion(true);
        
        Configuration conf2 = new Configuration();
        conf2.set("GENDER", args[5]);
        Job job2 = Job.getInstance(conf2,"student_gender");
        job2.setJarByClass(studentDetails2.class);
        job2.setMapperClass(testMap_gender.class);
        job2.setReducerClass(testReduce.class);
        
        job2.setOutputKeyClass(Text.class);
        job2.setOutputValueClass(IntWritable.class);
        
        FileInputFormat.addInputPath(job2,new Path(args[0]));
        FileOutputFormat.setOutputPath(job2, new Path(args[3]));
        
        job2.waitForCompletion(true);
        Configuration conf3 = new Configuration();
        conf3.set("INSTITUTE", args[5]);
        Job job3 = Job.getInstance(conf3,"student_instigender");
        job3.setJarByClass(studentDetails2.class);
        job3.setMapperClass(testMap_instigender.class);
        job3.setReducerClass(testReduce.class);
        
        job3.setOutputKeyClass(Text.class);
        job3.setOutputValueClass(IntWritable.class);
        
        FileInputFormat.addInputPath(job3,new Path(args[0]));
        FileOutputFormat.setOutputPath(job3, new Path(args[4]));
        
        job3.waitForCompletion(true);
        System.exit(job3.waitForCompletion(true)?0:1);
        
    }
}
```

### **Dataset: Temperature of Indian Cities. Fields of dataset are Date, Average Temperature, City, Country, Latitude and Longitude (Dataset is attached). Solve following questions**

1. Find maximum and minimum temperature of all cities from the given dataset
2. Count number of data point for each city.
3. Find the maximum and minimum temperature for city Bangalore from the given dataset.
4. Find the maximum and minimum temperature for any given city from the given dataset. City name should be passed through command line argument.

```java
import java.io.IOException;
import java.util.Iterator;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.FloatWritable;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.Mapper.Context;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class Weather {

	public static class testMapper extends Mapper<LongWritable,Text, Text, FloatWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
            String line = values.toString();
            String[] list = line.split(" ");
            
            
            context.write(new Text(list[2]),new FloatWritable(Float.parseFloat(list[1])));
   
    }}
	public static class testMapper_1 extends Mapper<LongWritable,Text,Text,IntWritable>
	{
		
		public void map(LongWritable key, Text values,Context context) throws IOException,InterruptedException
		{
			String line = values.toString();
			String entry[] = line.split(" ");
			
			
			context.write(new Text(entry[2]),new IntWritable(1));
		
		}
	}
	
	public static class testMapper_2 extends Mapper<LongWritable,Text, Text, FloatWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
           String line = values.toString();
           String[] list = line.split(" ");
           
           if(list[2].equals("Bangalore"))
           {
           
        	   context.write(new Text(list[2]),new FloatWritable(Float.parseFloat(list[1])));
    }}}
	
	public static class testMapper_3 extends Mapper<LongWritable,Text, Text, FloatWritable> {
        public void map(LongWritable key, Text values, Context context) throws IOException, InterruptedException{
        	String par = context.getConfiguration().get("CITY");
        	String line = values.toString();
            String[] city = line.split(" ");
            if(city[2].equals(par))
            {
            context.write(new Text(city[2]),new FloatWritable(Float.parseFloat(city[1])));
            }   
        }
	}

	public static class testReducer_2 extends Reducer<Text,IntWritable,Text,IntWritable>
	{
		public void reduce(Text key, Iterable <IntWritable> values,Context context) throws IOException,InterruptedException
		{
			int total_count = 0;
			for(IntWritable x:values) {
				total_count +=x.get();
			}
			context.write(key, new IntWritable(total_count));
		}
			
	}
	
	public static class testReduce extends Reducer<Text,FloatWritable,Text,FloatWritable> {
        public void reduce(Text key, Iterable<FloatWritable> values,Context context) throws IOException, InterruptedException{
        	float Max = 0,Min = 999;
        	
        	
			for(FloatWritable x: values) {
				if(Max < x.get())
				{
					Max = x.get();
				}
				if(Min > x.get())
				{
					Min = x.get();
				}
			}
			context.write(key, new FloatWritable(Max));
			context.write(key, new FloatWritable(Min));
        }
        
    }
	
	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
				Configuration conf = new Configuration();
				conf.set("CITY", args[5]);
				Job job1 = Job.getInstance(conf,"city-count1");
				job1.setJarByClass(Weather.class);
				job1.setMapperClass(testMapper.class);
				job1.setReducerClass(testReduce.class);
				job1.setOutputKeyClass(Text.class);
				job1.setOutputValueClass(FloatWritable.class);
				
				FileInputFormat.addInputPath(job1,new Path(args[0]));		
				FileOutputFormat.setOutputPath(job1,new Path(args[1]));
				job1.waitForCompletion(true);
				
				Configuration conf2 = new Configuration();
				Job job2 = Job.getInstance(conf2,"city-count2");
				job2.setJarByClass(Weather.class);
				job2.setMapperClass(testMapper_1.class);
				job2.setReducerClass(testReducer_2.class);
				job2.setOutputKeyClass(Text.class);
				job2.setOutputValueClass(IntWritable.class);
				
				FileInputFormat.addInputPath(job2,new Path(args[0]));		
				FileOutputFormat.setOutputPath(job2,new Path(args[2]));
				job2.waitForCompletion(true);
				
				Configuration conf3 = new Configuration();
				Job job3 = Job.getInstance(conf3,"city-count2");
				job3.setJarByClass(Weather.class);
				job3.setMapperClass(testMapper_2.class);
				job3.setReducerClass(testReduce.class);
				job3.setOutputKeyClass(Text.class);
				job3.setOutputValueClass(FloatWritable.class);
				
				FileInputFormat.addInputPath(job3,new Path(args[0]));		
				FileOutputFormat.setOutputPath(job3,new Path(args[3]));
				job3.waitForCompletion(true);
				
				Configuration conf4 = new Configuration();
				Job job4 = Job.getInstance(conf3,"city-count2");
				job4.setJarByClass(Weather.class);
				job4.setMapperClass(testMapper_2.class);
				job4.setReducerClass(testReduce.class);
				job4.setOutputKeyClass(Text.class);
				job4.setOutputValueClass(FloatWritable.class);
				
				FileInputFormat.addInputPath(job4,new Path(args[0]));		
				FileOutputFormat.setOutputPath(job4,new Path(args[4]));
				job4.waitForCompletion(true);
				
				System.exit(job4.waitForCompletion(true)?0:1);
	}
}
```

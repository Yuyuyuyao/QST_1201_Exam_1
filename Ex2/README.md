
题目2：编写MapReduce，统计`/user/hadoop/mapred_dev/ip_time` 中去重后的IP数，越节省性能越好。（35分）

---
import java.io.IOException;
import java.util.HashSet;
import java.util.Iterator;

import org.apache.hadoop.classification.InterfaceAudience.Public;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.Counters.Counter;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class Test {
		public static class map extends Mapper<LongWritable, Text, Text, Text>{

			@Override
			protected void map(LongWritable key, Text value, Context context)
					throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				String line = value.toString();
				String[] aString = line.split(" ");
				HashSet<String> hashSet = new HashSet<String>();
				for(int i=0;i<aString.length;i++){
					hashSet.add(aString[i]);
				}
				for (String string : hashSet) {
					context.write(new Text(string),new Text("1"));
					
				}	
				
				
			}
			
		}
		public static class reduce extends Reducer<Text, Text, Text, Text>{
			private static int cou =0;
			public static void cleanUp(Context context){
				// TODO Auto-generated method stub
				Counter counter = (Counter) context.getCounter("Custom Static", "Unique IP");
				System.out.println("This has "+cou);
				counter.increment(cou);
			}
			@Override
			protected void reduce(Text key, Iterable<Text> values, Context context)
					throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				cou+= 1;
				context.write(null, null);
			}
			
		} 
		
		public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
			  Configuration conf =new Configuration();
			   Job job =Job.getInstance(conf,"IP");
			   job.setJarByClass(map.class);
			   job.setMapperClass(map.class);
			   job.setMapOutputKeyClass(Text.class);
			   job.setMapOutputValueClass(Text.class);
			   FileInputFormat.addInputPath(job, new Path(args[0]));
			   FileOutputFormat.setOutputPath(job, new Path(args[1]));
			   job.waitForCompletion(true);
			   Long linenumber=job.getCounters().findCounter("org.apache.hadoop.mapreduce.TaskCounter","REDUCE_OUTPUT_RECORDS").getValue();
		}
		
}

运行完之后，描述程序里所做的优化点，每点+5分。

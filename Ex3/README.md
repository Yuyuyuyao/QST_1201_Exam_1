题目3：编写MapReduce，统计这两个文件

`/user/hadoop/mapred_dev_double/ip_time`

`/user/hadoop/mapred_dev_double/ip_time_2`

当中，重复出现的IP的数量(40分)
package test3;

import java.io.IOException;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

public class Test3 {
	public sttic class map extends Mapper<LongWritable, Text, Text, Text>{

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
		public void setup(Context context){
			Configuration configuration = new Configuration();
			FileSystem fileSystem = FileSystem.get(conf)
		}
		protected void reduce(Text key, Iterable<Text> values, Context context)
				throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			cou+= 1;
			context.write(null, null);
		}
		
	} 
			
		}
		
	}
	public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
      Configuration conf =new Configuration();
       Job job =Job.getInstance(conf,"IP");
       job.setJarByClass(map.class);
       job.setMapperClass(map.class);
       job.setMapOutputKeyClass(Text.class);
       job.setMapOutputValueClass(Text.class);
       job.setOutputKeyClass(Text.class);
       job.setOutputValueClass(Text.class);
       FileInputFormat.addInputPath(job, new Path(args[0]));
       FileOutputFormat.setOutputPath(job, new Path(args[1]));
       job.waitForCompletion(true);
       Long linenumber=job.getCounters().findCounter
       ("org.apache.hadoop.mapreduce.TaskCounter","REDUCE_OUTPUT_RECORDS").getValue();
    }
}
---
加分项：

1. 写出程序里面考虑到的优化点，每个优化点+5分。
2. 额外使用Hive实现，+5分。
3. 额外使用HBase实现，+5分。

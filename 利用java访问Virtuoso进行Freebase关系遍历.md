# Virtuoso数据抽取java版API

由于结果太大，直接访问virtuoso界面查询会time out，

在终端执行时能输出结果，但是由于不熟悉SQL下如何重定向输出到文件中也没有成功得到所有relations，如下：

```sql lite
$isql-vt
SQL>sparql select distinct ?y where{?x ?y ?z};
```

[这里是一个java写的可以直接访问virtuoso查询的程序](http://vos.openlinksw.com/owiki/wiki/VOS/VirtJenaSPARQLExample1)

```java

import com.hp.hpl.jena.query.*;
import com.hp.hpl.jena.rdf.model.RDFNode;

import virtuoso.jena.driver.*;

public class VirtuosoSPARQLExample1 {

	/**
	 * Executes a SPARQL query against a virtuoso url and prints results.
	 */
	public static void main(String[] args) {

		String url;
		if(args.length == 0)
		    url = "jdbc:virtuoso://10.109.247.135:1111";//此次改为自己的ip
		else
		    url = args[0];

/*			STEP 1			*/
		VirtGraph set = new VirtGraph (url, "dba", "dba");

/*			STEP 2			*/


/*			STEP 3			*/
/*		Select all data in virtuoso	*/
		Query sparql = QueryFactory.create("SELECT * WHERE { GRAPH ?graph { ?s ?p ?o } } limit 100");

/*			STEP 4			*/
		VirtuosoQueryExecution vqe = VirtuosoQueryExecutionFactory.create (sparql, set);

		ResultSet results = vqe.execSelect();
		while (results.hasNext()) {
			QuerySolution result = results.nextSolution();
		    RDFNode graph = result.get("graph");
		    RDFNode s = result.get("s");
		    RDFNode p = result.get("p");
		    RDFNode o = result.get("o");
		    System.out.println(graph + " { " + s + " " + p + " " + o + " . }");
		}
	}
}

--------------------------------------------------------------------------------
 

© Copyright OpenLink Software  2016- 
 OpenLink Software Documentation Team <virtuoso.docs@openlinksw.com> 
    
```



[在这里下载相应的jar包](http://vos.openlinksw.com/owiki/wiki/VOS/VOSDownload#Jena%20Provider)

![QQ图片20170718114446](C:\Users\ls\Desktop\QQ图片20170718114446.png) 

然后就可以看到ralations愉快的打印粗来了@@

![QQ图片20170718114536](C:\Users\ls\Desktop\QQ图片20170718114536.png)


# ot-notes
Some notes on Open Targets

### Searching OT P ES instance with elastic4s
Tricky because elastic4s examples are for ES 6.x, not 7.x.

In a Jupyter notebook with the Almond kernel, assuming you're running an ES instance with the OT P indices loaded and serving on `localhost:9200`
```scala
import $ivy.`com.sksamuel.elastic4s::elastic4s-core:7.3.4`
import $ivy.`com.sksamuel.elastic4s::elastic4s-client-esjava:7.3.4`

import com.sksamuel.elastic4s.http.JavaClient
import com.sksamuel.elastic4s.{ElasticClient, ElasticProperties}
import com.sksamuel.elastic4s.ElasticDsl._

val client = ElasticClient(JavaClient(ElasticProperties("http://localhost:9200")))

// Remind myself of index names
val indexes = client.execute {
  catIndices()
}.await.result.map(_.index)

// Example search
val resp = client.execute {
  search("19.11_gene-data") query "DNASE1L3"
}.await.result.hits.hits.map(_.sourceAsMap("entrez_gene_id"))
```

# corteza-examples
Examples for corteza use cases


Here are some use case examples for Corteza

1. Invoking an external workflow to write to a queue (with consumptions by a second workflow)

* The write to queue is invoked by an external http request, like so:

Step 1. Obtain a login token (see login png) and stored JWT token (I am using postman, so save it to context - i.e. in tests I place this:

var jsonData = JSON.parse(responseBody);
postman.setGlobalVariable("access_token", jsonData.access_token);

Step 2. Post to the workflow which writes to the queue

POST http://localhost:808/automation/workflows/339721397272051813/exec

Headers:
- Authorization (your Corteza login token)

JSON Body:

{
  "stepID": "4",  
  "trace": false,
  "wait": false,
  "async": false,
  "input": {              
        "request":         
            { "@Value": ["{\"quoteId\": \"339723775660654693\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"Commission\",\"iv_installment\":\"O\",\"iv_value\": \"7.99\",\"iv_desc\":\"PrimaAnualComisioBruta\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"Commission\",\"iv_value_type\":\"commission\",\"iv_value_basis\":\"gross\"}",
                "{\"quoteId\": \"339558683325400757\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"IRPF\",\"iv_installment\":\"O\",\"iv_value\": \"0.10\",\"iv_desc\":\"Comissio\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"IRPF\",\"iv_value_type\":\"tax\",\"iv_value_basis\":\"gross\"}",
                "{\"quoteId\": \"339723775660654693\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"Commission\",\"iv_installment\":\"O\",\"iv_value\": \"7.99\",\"iv_desc\":\"PrimaAnualComisioBruta\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"Commission\",\"iv_value_type\":\"commission\",\"iv_value_basis\":\"gross\"}",
                "{\"quoteId\": \"339723775660654693\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"Commission\",\"iv_installment\":\"O\",\"iv_value\": \"7.99\",\"iv_desc\":\"PrimaAnualComisioBruta\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"Commission\",\"iv_value_type\":\"commission\",\"iv_value_basis\":\"gross\"}",
                "{\"quoteId\": \"339723775660654693\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"Commission\",\"iv_installment\":\"O\",\"iv_value\": \"7.99\",\"iv_desc\":\"PrimaAnualComisioBruta\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"Commission\",\"iv_value_type\":\"commission\",\"iv_value_basis\":\"gross\"}",
                "{\"quoteId\": \"339723775660654693\",\"packId\": \"339722786862006373\",\"iv_unique_code\":\"Commission\",\"iv_installment\":\"O\",\"iv_value\": \"7.99\",\"iv_desc\":\"PrimaAnualComisioBruta\",\"DeltaKey\":\"top_TripCancellationCost.c3000\",\"iv_name\":\"Commission\",\"iv_value_type\":\"commission\",\"iv_value_basis\":\"gross\"}"],
              "@Type": "Array"
            }
    }
}


* Note that it is necessary to escape the JSON so that it parses correctly in Corteza, otherwise issues.

This post request invokes the workflow with the ID 339721397272051813 on step 4, once you create it in your system the ID will be different (easy to find, open the workflow and check the ID in the URL BAR)

The workflow called write_to_queue [write to queue](write_to_queue.png) is invoked and the array passed. It processes each item and writes a json string to a queue called QuoteLineUpdates

On the otherside, workflow called [consumer](consumer.png) picksup each string from the queue and with JSON.parse(input) obtain a json representation.

You can import the workflows directly:
* [Write to queue](https://github.com/mperestrelo/corteza-examples/blob/main/WriteToQueue.json)
* [Consumer](https://github.com/mperestrelo/corteza-examples/blob/main/Consumer.json)



Note that:
* Depending on what record you want to create you will need to customize the namespace and module handles etc

+++
title = 'Airflow Documentation'
date = 2023-12-21T18:31:53-05:00
draft = false
+++

* These notes are taken from the official Apache Airflow website [Documentation](https://airflow.apache.org/docs/apache-airflow/stable/index.html) section. Will possibly need to reference these notes later.
* "**Apache Airflow** is an open source platform for developing, scheduling, and monitoring batch-oriented workflows. Airflow’s extensible Python framework enables you to build workflows connecting with virtually any technology. A web interface helps manage the state of your workflows. Airflow is deployable in many ways, varying from a single process on your laptop to a distributed setup to support even the biggest workflows."

# [Fundamental Concepts](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/fundamentals.html)
"An Airflow pipeline is just a Python script that happens to define an Airflow DAG object."

**Example DAG**
```python
import textwrap
from datetime import datetime, timedelta
from airflow.models.dog import DAG
from airflow.operators.bash import BashOperator

with DAG(
   "tutorial",
   default_args = {
       "depends_on_past": False,
       "email": ["airflow@example.com"],
       "email_on_failure": False,
       "email_on_retry": False,
       "retries": 1,
       "retry_delay": timedelta(minutes = 5),
   },
   description = "A simple tutorial DAG",
   schedule = timedelta(days = 1),
   start_date = datetime(2021, 1, 1),
   catchup = False,
   tags = ["example"],
) as dag:
   t1 = BashOperator(
       task_id = "print_date",
       bash_command = "date",
   )
   t2 = BashOperator(
       task_id = "sleep",
       depends_on_past = False,
       bash_command = "sleep 5",
       retries = 3,
   )
   t1.doc_md = textwrap.dedent(
       """\
   #### Task Documentation
   You can document your task using the attributes `doc_md` (markdown),
   `doc` (plain text), `doc_rst`, `doc_json`, `doc_yaml` which gets
   rendered in the UI's Task Instance Details page.
   ![img](http://montcs.bloomu.edu/~bobmon/Semesters/2012-01/491/import%20soul.png)
   **Image Credit:** Randall Munroe, [XKCD](https://xkcd.com/license.html)
   """
   )
   dag.doc_md = __doc__
   dag.doc_md = """
   This is a documentation placed anywhere
   """
   templated_command = textwrap.dedent(
       """
   {% for i in range(5) %}
       echo "{{ ds }}"
       echo "{{ macros.ds_add(ds, 7)}}"
   {% endfor %}
   """
   )

   t3 = BashOperator(
       task_id="templated",
       depends_on_past=False,
       bash_command=templated_command,
   )

   t1 >> [t2, t3]
```

**Command Line Metadata Validation**
```python
# initialize the database tables
airflow db migrate


# print the list of active DAGs
airflow dags list


# prints the list of tasks in the "tutorial" DAG
airflow tasks list tutorial


# prints the hierarchy of tasks in the "tutorial" DAG
airflow tasks list tutorial --tree
```

**Command Layout**
```python
# command layout: command subcommand [dag_id] [task_id] [(optional) date]
```

# [Working with TaskFlow](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html)

**Example "TaskFlow API" Pipeline**
```python
import json
import pendulum
from airflow.decorators import dag, task

@dag(
   schedule=None,
   start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
   catchup=False,
   tags=["example"],
)

def tutorial_taskflow_api():
   @task()
   def extract():
       data_string = '{"1001": 301.27, "1002": 433.21, "1003": 502.22}'

       order_data_dict = json.loads(data_string)
       return order_data_dict
  
   @task(multiple_outputs=True)
   def transform(order_data_dict: dict):
       total_order_value = 0

       for value in order_data_dict.values():
           total_order_value += value

       return {"total_order_value": total_order_value}
  
   @task()
   def load(total_order_value: float):
       print(f"Total order value is: {total_order_value:.2f}")
  
   order_data = extract()
   order_summary = transform(order_data)
   load(order_summary["total_order_value"])

tutorial_taskflow_api()
```
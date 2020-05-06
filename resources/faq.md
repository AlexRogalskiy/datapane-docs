# FAQ

### Where are my scripts hosted and run?

Datapane provides a public instance for individuals, private instances for teams, or an on-premise deployment which you can run on your own infrastructure.

### Can scripts on Datapane talk to databases and APIs?

Yes. Once on Datapane, your code has network access and can connect to third-party systems, so can push or pull from a database or API.

### I already have data infrastructure for resource intensive tasks, like model training. Do I have to run this on Datapane?

No. Datapane's philosophy is to fit into your existing infrastructure, and provides an API to push assets \(such as trained models\) from other platforms so you can access them in your scripts.

### What libraries can my script use on Datapane?

Datapane supports the majority of Python visualisation libraries, such as Bokeh, Altair, and Matplotlib, and many Python analysis libraries. You can also provide your own requirements from pip or local libraries.

### Can I generate static reports from Airflow, a hosted Jupyter, or my local Python environment?

Yes. If you don't need to interactive scripts which generate reports dynamically, you can create reports directly from your existing Python environment, without running code on Datapane.

### Where can I embed reports?

In addition to iframe support, you can embed reports directly into Reddit, Medium, Notion, and more.


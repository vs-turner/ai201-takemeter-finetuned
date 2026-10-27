# Staff labels were never provided

`data/staff_labels.csv` is not in this repo. The TF sends that file after `my_staff_labels.csv` is saved. It was not sent. `agreement.py` cannot run, and there is no agreement rate and no disagreement list.

Do not fill that gap with made-up labels on the 30 posts in `data/staff_posts.csv`. An agreement report written against invented labels would claim staff said things staff did not say.

`my_staff_labels.csv` is already committed. When the real file arrives, put it at `data/staff_labels.csv` and run `python agreement.py`. The two files join on `id`. The label column may be named `label`.

The shape, using posts that are not in the staff set:

```csv
id,label
example_001,analysis
example_002,hot_take
example_003,reaction
```

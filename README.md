# python-for-powerbi

## To install
```
pip install pypowerbi
```

## To post a data set
```
import adal
from pypowerbi.dataset import Column, Table, Dataset
from pypowerbi.client import PowerBIClient

authority_url = 'https://login.windows.net/common'
resource_url = 'https://analysis.windows.net/powerbi/api'
api_url = 'https://api.powerbi.com'
```
### Credentials
```
client_id = '00000000-0000-0000-0000-000000000000'
username = 'someone@somecompany.com'
password = 'averygoodpassword'
```
### Authenticate 
```
context = adal.AuthenticationContext(authority=authority_url,
                                     validate_authority=True,
                                     api_version=None)
```
### Authentication token
```
token = context.acquire_token_with_username_password(resource=resource_url,
                                                     client_id=client_id,
                                                     username=username,
                                                     password=password)
```
### Powerbi api client
```client = PowerBIClient(api_url, token)```

### Create columns
```
columns = []
columns.append(Column(name='id', data_type='Int64'))
columns.append(Column(name='name', data_type='string'))
columns.append(Column(name='is_interesting', data_type='boolean'))
columns.append(Column(name='cost_usd', data_type='double'))
columns.append(Column(name='purchase_date', data_type='datetime'))
```

### Create tables
```
tables = []
tables.append(Table(name='AnExampleTableName', columns=columns))
```

### Create dataset
```dataset = Dataset(name='AnExampleDatasetName', tables=tables)```

### Post dataset
```client.datasets.post_dataset(dataset)```

## To refresh a dataset
```
import adal
from pypowerbi.dataset import Column, Table, Dataset
from pypowerbi.client import PowerBIClient
from Credentials import client_id, username, password
```

### The Report to Refresh
```
group_id = "<your dataset's group id>"
dataset_id = "<your dataset's report id>"
```

### Powerbi api client
```
client = PowerBIClient.get_client_with_username_password(client_id=client_id, username=username, password=password)

notify_option = "MailOnFailure"
client.datasets.refresh_dataset(dataset_id, notify_option=notify_option, group_id=group_id)
```

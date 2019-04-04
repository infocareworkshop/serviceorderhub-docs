# Shipping

## GET /api/v1/shipping/label

Get shipping label (Base64 encoded PDF file).

### Input:

| Name   | Type   | Description   |
|--------|--------|---------------|
| guid   | GUID   | Case's GUID   |


### Output:

*(Shown with wrapper object)*

```
{
  "data": "dataJVBERi0xLjQNCiXi48/TDQo..."
}
```
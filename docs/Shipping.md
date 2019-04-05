# Shipping

## GET /api/v2/shipping/label

Get shipping label (Base64 encoded PDF file).

### Access

Partner, Provider

### Input

| Name   | Type   | Description   |
|--------|--------|---------------|
| guid   | GUID   | Case's GUID   |


### Output

*(Shown with wrapper object)*

```
{
  "data": "dataJVBERi0xLjQNCiXi48/TDQo..."
}
```
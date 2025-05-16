# HubSpot Multi-Property Mapper

A custom code solution for mapping multi-select property values from deals to their associated company properties in HubSpot workflows.

## Overview

This script takes values from a deal's multi-select property and maps them to specific values in company properties. It handles the conversion between displayed labels and internal values, ensuring proper data synchronization between different HubSpot objects.

## Current Configuration

The script is currently configured to:

1. Read from the deal property `multi_deal_prop` (a multi-select field with options 1-8)
2. Map values 1-4 to the company property `company_multi_prop_1`
3. Map values 5-8 to the company property `company_multi_prop_2`

## How to Use

1. Create a workflow in HubSpot that triggers on deal creation or property update
2. Add a "Custom Code" action to your workflow
3. Copy and paste the script into the custom code editor
4. Configure the mapping as needed (see below)
5. Save and activate your workflow

## How to Customize

### Changing Property Names

To use different property names, modify these constants at the top of the script:

```python
# Configuration
DEAL_PROPERTY = "multi_deal_prop"        # Change to your deal property name
COMPANY_PROP_1 = "company_multi_prop_1"  # Change to your first company property
COMPANY_PROP_2 = "company_multi_prop_2"  # Change to your second company property
```

### Changing the Mapping Logic

The mapping between deal values and company property values is defined in the `MAPPINGS` dictionary:

```python
MAPPINGS = {
    # Deal values -> company_multi_prop_1 values
    "1": {"property": COMPANY_PROP_1, "value": "1"},  # Map deal "1" to company prop 1 value "1"
    "2": {"property": COMPANY_PROP_1, "value": "2"},  # Map deal "2" to company prop 1 value "2"
    "3": {"property": COMPANY_PROP_1, "value": "3"},  # Map deal "3" to company prop 1 value "3" 
    "4": {"property": COMPANY_PROP_1, "value": "4"},  # Map deal "4" to company prop 1 value "4"
    
    # Deal values -> company_multi_prop_2 values
    "5": {"property": COMPANY_PROP_2, "value": "1"},  # Map deal "5" to company prop 2 value "1"
    "6": {"property": COMPANY_PROP_2, "value": "2"},  # Map deal "6" to company prop 2 value "2"
    "7": {"property": COMPANY_PROP_2, "value": "3"},  # Map deal "7" to company prop 2 value "3"
    "8": {"property": COMPANY_PROP_2, "value": "4"},  # Map deal "8" to company prop 2 value "4"
}
```

## Example Mapping Scenarios

### Scenario 1: Map to a single company property

```python
MAPPINGS = {
    "1": {"property": "company_category", "value": "tier_1"},
    "2": {"property": "company_category", "value": "tier_2"},
    "3": {"property": "company_category", "value": "tier_3"},
}
```

### Scenario 2: Map to three different company properties

```python
MAPPINGS = {
    "1": {"property": "product_interest_a", "value": "true"},
    "2": {"property": "product_interest_b", "value": "true"},
    "3": {"property": "product_interest_c", "value": "true"},
}
```

### Scenario 3: Map to boolean properties

```python
MAPPINGS = {
    "interested_product_a": {"property": "product_a_interest", "value": "true"},
    "not_interested_product_a": {"property": "product_a_interest", "value": "false"},
}
```

## Important Notes

1. **Internal Values vs. Display Labels**: HubSpot properties have both display labels (what users see) and internal values (what's stored in the database). Make sure you're mapping to the correct internal values.

2. **Finding Internal Values**: 
   - In HubSpot, go to Settings > Properties
   - Find the company property you want to map to
   - Look at the options to see the label and internal value pairs

3. **Multi-Select Property Format**: Multi-select values in HubSpot are stored as semicolon-separated strings (e.g., "1;3;5").

## Workflow Output

The script returns two output fields that can be used in your workflow:

- `mapping_success`: Boolean indicating whether at least one company was successfully updated
- `companies_updated`: Number of companies that were successfully updated

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

# Optimize TikTok Ad Campaigns

This page guides you through how to optimize TikTok ad campaigns by pulling the target, creative, bidding, and budget levers with Marketing API.

## Prerequisites

Before proceeding, make sure that you meet the following requirements:
1. Create a TikTok For Business account.
2. Register as a developer.
3. Create a developer application.
4. Get authorization from advertisers.
5. Get the access token.  

For more information, see [Getting Started](#https://ads.tiktok.com/marketing_api/docs?id=1702715749213185).


## Targeting optimization

### Enable automated targeting

Automated Targeting is a feature that allows you to optimize your target audience at the ad group level.

With this feature, TikTok For Business platform dynamically will select the most relevant audience based on ad group information at different delivery phases. This drives optimal conversions.

To enable the automated targeting feature, set `automated_targeting` to `ON` in the following scenarios:
- Call [`/adgroup/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708503506967554) to create an ad group
- Call [`/adgroup/update/`](https://ads.tiktok.com/marketing_api/docs?id=1708530959460353) to update an ad group


### Conduct split testing for targeting

Split test, commonly referred to as A/B testing, is a tool that allows you to test different versions of your ads to determine which one works best.

When creating a split test for targeting, the targeting settings for the two ad groups must be different, while the ads under the two ad groups must be the same.

To conduct a split test for targeting, perform the following steps:

#### Step 1: Create a split test
1. Create a campaign  
When calling [`/campaign/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708583041422337) endpoint to create a campaign, set `split_test_variable` to `TARGETING` in the API request to specify target audience for the split test variable.

2. Get estimated numbers <a name="estimateid"></a>   
Call [`/split_test/estimate/`](https://ads.tiktok.com/marketing_api/docs?id=1701890940286977) endpoint to obtain the estimated information such as audience number, proposed budget, and estimated power value.  
You can modify the variable conditions and bids based on the response, and have a minimum of 80% power value to ensure that the split test can obtain reasonable test results. 

3. Create ad groups  
Call [`/adgroup/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708503506967554) to create ad groups for the split test and specify the following fields:

| Field | Data Type | Description |
|:--------|:-------|:-----------------|
|`is_split_test`| bool | Whether to create ad groups for split test:<li>`True`: set to `True` for the split test<li>`False`: the default value |
|`split_test_estimate_id`| number | Pass the estimate ID returned in the [Get estimated numbers](#estimateid) request. |
|`targeting_variable`|object| Set different targeting values to compare how effective your ads are reaching different audience or demographics.|

4. Create ads  
Call [`/ad/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708573052311553) to create ads for the split test and specify the following fields:

| Field | Data Type | Description |
|:--------|:-------|:-----------------|
|`is_split_test`| bool | Whether to create ad groups for split test:<li>`True`: set to `True` for the split test <li>`False`: the default value |
|`adgroup_ids`| number | Set two ad group IDs of a split test campaign. <br> **Note**: Create the same ads for the specified ad groups. |

#### Step 2: Get split test results

After the split test is complete, call [`/split_test/result/get/`](https://ads.tiktok.com/marketing_api/docs?id=1701890940791810) to obtain the split test results and get the ID of the winning ad group based on different indicators.

#### Step 3: Run winning ad groups

After obtaining the test results, call [`/split_test/promote/`](https://ads.tiktok.com/marketing_api/docs?id=1701890941323265) to pause the losing ad group and solely run the winning ad group.

## Creative optimization

### Enable automated creative optimization

Automated Creative Optimization (ACO) is a feature that allows you to optimize your budget at the ad group level.

With this feature, TikTok For Business platform will automatically combine your creative assets into multiple ads for your campaign. These ads get continuously explored, evaluated, and optimized to find the optimal combination of variables. TikTok For Business platform will then presents the best creative to your target audience based on the tested combinations.

To enable the ACO feature, set `creative_material_mode` to `DYNAMIC` when creating an ad group ([`/adgroup/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708503506967554)).

### Conduct split testing for creatives

> **Note**: The Automated Creative Optimization feature is not supported for split testing.
    
When creating a split test for creatives, the ad group settings must be the same, while the ads settings under the two ad groups must be different.

To conduct a split test for creatives, perform the following steps:

#### Step 1: Create a split test
1. Create a campaign  
When calling [`/campaign/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708583041422337) endpoint to create a campaign, set `split_test_variable` to `CREATIVE` in the API request to specify creatives for the split test variable.

2. Get estimated numbers <a name="estimateid1"></a>   
Call [`/split_test/estimate/`](https://ads.tiktok.com/marketing_api/docs?id=1701890940286977) endpoint to obtain the estimated information such as audience number, proposed budget, and estimated power value.  
You can modify the variable conditions and bids based on the response, and have a minimum of 80% power value to ensure that the split test can obtain reasonable test results. 

3. Create ad groups  
Call [`/adgroup/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708503506967554) to create ad groups for the split test and specify the following fields:

| Field | Data Type | Description |
|:--------|:-------|:-----------------|
|`is_split_test`| bool | Whether to create ad groups for split test:<li>`True`: set to `True` for the split test<li>`False`: the default value |
|`split_test_estimate_id`| number | Pass the estimate ID returned in the [Get estimated numbers](#estimateid1) request. |

4. Create ads  
Call [`/ad/create/`](https://ads.tiktok.com/marketing_api/docs?id=1708573052311553) to create ads for the split test and specify the following fields:

| Field | Data Type | Description |
|:--------|:-------|:-----------------|
|`is_split_test`| bool | Whether to create ad groups for split test:<li>`True`: set to `True` for the split test <li>`False`: the default value |
|`adgroup_id`| object | Set two ad group IDs of a split test campaign under the `creatives` field.<br> **Note**: Create two different ads under the specified ad groups separately. |

#### Step 2: Get split test results

After the split test is complete, call [`/split_test/result/get/`](https://ads.tiktok.com/marketing_api/docs?id=1701890940791810) to obtain the split test results and get the ID of the winning ad group based on different indicators.

#### Step 3: Run winning ad groups

After obtaining the test results, call [`/split_test/promote/`](https://ads.tiktok.com/marketing_api/docs?id=1701890941323265) to pause the losing ad group and solely run the winning ad group.


## Budget optimization

### Enable campaign budget optimization

Campaign Budget Optimization (CBO) is a feature that allows you to optimize your budget at the campaign level. With this feature, you can apply a single set of budget optimizations to all the ad groups that belong to a campaign rather than setting them up individually.

To enable the CBO feature, set `budget_optimize_switch` to `1` when creating a campaign ([`/campaign/create`/](https://ads.tiktok.com/marketing_api/docs?id=1708583041422337)).

Once CBO is enabled for a campaign, the available values of the following fields change accordingly:
- `bid_type`: Only `BID_TYPE_NO_BID` is supported.
- `budget_mode`: Only `BUDGET_MODE_DAY` is supported.
- `objective_type`: Only `APP_INSTALL`, `CONVERSIONS`, `ENGAGEMENT`, `TRAFFIC`, `REACH`, `VIDEO_VIEWS`, `LEAD_GENERATION`, and `CATALOG_SALES` are supported.

> **Note:**  
    When creating an ad group in this campaign, you do not need to specify `bid_type`, `budget_mode`, `budget`, `optimize_goal`, `pacing` as the settings for these parameters are inherited from the campaign.  
    Besides, we recommend that you use the same bid method for all ad groups in the campaign.


## Bidding optimization

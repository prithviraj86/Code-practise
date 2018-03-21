# Code-practise
Query:-

select sku,COUNT(*),CONCAT(
                        '[',
                          GROUP_CONCAT(
                              CONCAT(
                                '{',
                                	'"vraiant_id":'   , '"', variant_id   , '"', ','
                                   	 '"product_name":', '"', product_title, '"', ','
                                   	 '"title":', '"', title, '"', ','
                                   	 '"quantity":', '"', quantity, '"', ','
                                   	 '"price":', '"', price, '"', ','
                                   	'"exclude":', '"', exclude, '"', ','
                                	'"created_at":', '"'  , created_at, '"', ','
                                	'"updated_at":', '"'  , updated_at,'"',
                               				'}'
                              ),
                          ''),
                        ']'
                        ) as valid_json from `variants` group by `sku` having COUNT(*) > 1
Output:-
	
[
{"vraiant_id":"12345353545","quantity":"10","created_at":"2018-03-09 18:39:25","updated_at":"2018-03-09 18:39:25"},
{"vraiant_id":"46638865456","quantity":"8","created_at":"2018-03-09 18:39:25","updated_at":"2018-03-09 18:39:25"},
{"vraiant_id":"89789789789","quantity":"0","created_at":"2018-03-09 18:39:25","updated_at":"2018-03-09 18:39:25"}
]

Limitations :-
GROUP_CONCAT have 1024 characters limits so we will increse the limit also using these queries
	For Session:-
		SET SESSION group_concat_max_len = 1000000;
	For Global:-
		SET GLOBAL group_concat_max_len = 1000000;

Other method:-
This method only work in MYSQL 5.7

CONCATE(
'[',
GROUP_CONCAT(
	JSON_OBJECT('vraiant_id', variantid ,'quantity',quantity,'created_at',created_at,'updated_at', updated_at)
	),
']'
)

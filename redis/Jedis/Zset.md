# Zset操作

- 查询
```java
        String recordKey = KeyGen.buildLukcyRecordZSetKey();
		Set<String> recordSet = redisTemplate.opsForZSet().reverseRange(recordKey, 0, Constants.MAX_RECENT_RECORD_COUNT);
		if (CollectionUtils.isEmpty(recordSet)) {
			return Collections.emptyList();
		} else {
			List<LuckyRecord> totalRecords = new ArrayList<>(recordSet.size());
			for (String recordMember : recordSet) {
				String[] splitedString = recordMember.split(Constants.DELEMITER);
				LuckyRecord record = new LuckyRecord();
				record.setUid(Long.valueOf(splitedString[0]));
				record.setPrize(Integer.valueOf(splitedString[1]));
				totalRecords.add(record);
			}
			if (totalRecords.size() <= Constants.PICKEND_RECORD_COUNT) {
				return totalRecords;
			} else {
				// 如果大于20条，那么抽出最大的前20条
				return totalRecords.subList(0, Constants.PICKEND_RECORD_COUNT);
			}
		}


```
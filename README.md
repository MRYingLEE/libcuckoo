I will post my code here at first, then I will try to make pull requests to libcuckoo (https://github.com/efficient/libcuckoo), the original project.

libcuckoo
=========

libcuckoo provides a high-performance, compact hash table that allows
multiple concurrent reader and writer threads.

libcuckoo-transaction
=========
Bascially, so far, libcuckoo has 4 basic operations: find, update, delete, insert.

This project aims to bring libcuckoo light transaction functions. So that you can rely libcuckoo to deal with concurrent map operations, and don't need to pay attention on lock and concurrency any more.

So far, I only focus on one pair light transaction functions. Generally, the light transactions only consists not more than 3 basic operations.

A basic light transaction concept chart is as the following:
		
		      If key-value pair exists
					call exists_function (check/update/delete possible only)
				else (not exists)
					call not_exists_function (insert possible only)

Besides the updater function type, I defined checker function type, which is an updater with a bool return value.

Roadmap
=========
	Level 1: single operation with flexible API <Implemented>
		find (existing in original project) / find_fn (only valid value returns, implemented in this project) ;
		update (existing in original project) / update_fn (complex update, existing in original project);
		erase (existing in original project) / erase_fn (conditional erase, implemented in this project);
	
	Level 2: fixed combinations <Implemented>
		exists_function (check/update, delete) + not_exists_function (insert) 
	
		upsert=check/update + insert (existing in original project) 
		update_or_insert=update + insert
		find_or_insert=find + insert
		delete_or_insert= delete + insert
		
	Level 3: Flexible Combinations <Implemented>
		deleter
		deleter_or_insert
		
	Level 4: Flexible Combinations <Maybe>
		to make basic operations are public. And programmer can make flexible logical     

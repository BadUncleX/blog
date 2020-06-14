---
layout: post
title:  " how to write tests in substrate "
date:   2020-06-14 21:15:56 +0800
categories: substrate 
math: true
typora-copy-images-to: ../assets/images/2020-06
typora-root-url: ../../blog_alexcode
---

detail commit from here: [Github Commit](https://github.com/alexwanng/substratecourse/commit/16a7aca8f3b5e4269c9ea4ca683b483a59dadee1#diff-8fc86bb083969aededb1ea8b82b3c429L31-R69)



git command from: 

```bash
git diff-tree -p 16a7aca8f3b5e4269c9ea4ca683b483a59dadee1
```



result: 



```diff
16a7aca8f3b5e4269c9ea4ca683b483a59dadee1
diff --git a/lesson3/substrate-node-template/pallets/poe/src/mock.rs b/lesson3/substrate-node-template/pallets/poe/src/mock.rs
index 951e468..f1ca265 100644
--- a/lesson3/substrate-node-template/pallets/poe/src/mock.rs
+++ b/lesson3/substrate-node-template/pallets/poe/src/mock.rs
@@ -31,6 +31,7 @@ impl system::Trait for Test {
 	type BlockNumber = u64;
 	type Hash = H256;
 	type Hashing = BlakeTwo256;
+	// 注意这两个声明 AccountId 和 Lookup
 	type AccountId = u64;
 	type Lookup = IdentityLookup<Self::AccountId>;
 	type Header = Header;
@@ -49,12 +50,13 @@ impl system::Trait for Test {
 	type OnNewAccount = ();
 	type OnKilledAccount = ();
 }
-
+// 测试需要用到的常量声明
 parameter_types! {
 	pub const MaxClaimLength: u32 = 6;
 }
 impl Trait for Test {
 	type Event = ();
+	// 测试需要用到的常量声明
 	type MaxClaimLength = MaxClaimLength;
 }
 pub type PoeModule = Module<Test>;
@@ -62,6 +64,7 @@ pub type PoeModule = Module<Test>;
 
 // This function basically just builds a genesis storage key/value store according to
 // our desired mockup.
+// test 里用到的基本函数库， 公用
 pub fn new_test_ext() -> sp_io::TestExternalities {
 	system::GenesisConfig::default().build_storage::<Test>().unwrap().into()
 }
diff --git a/lesson3/substrate-node-template/pallets/poe/src/tests.rs b/lesson3/substrate-node-template/pallets/poe/src/tests.rs
index 83da5e7..bff9831 100644
--- a/lesson3/substrate-node-template/pallets/poe/src/tests.rs
+++ b/lesson3/substrate-node-template/pallets/poe/src/tests.rs
@@ -8,9 +8,13 @@ use super::*;
 #[test]
 fn create_claim_works() {
 
+    // 测试中使用mock中声明的new_test_ext
     new_test_ext().execute_with(|| {
         let claim = vec![0, 1];
+        // 执行成功的操作， 使用 assert_ok!
         assert_ok!(PoeModule::create_claim(Origin::signed(1), claim.clone()));
+
+        // 逻辑相等操作， 使用 assert_eq!
         assert_eq!(Proofs::<Test>::get(&claim), (1, system::Module::<Test>::block_number()));
     })
 }
@@ -23,6 +27,7 @@ fn create_claim_failed_when_claim_already_exists() {
         let claim = vec![0, 1];
         let _ = PoeModule::create_claim(Origin::signed(1), claim.clone());
 
+        // 预期返回一个指定的错误， 使用 assert_noop!
         assert_noop!(
             PoeModule::create_claim(Origin::signed(1), claim.clone()),
             Error::<Test>::ProofAlreadyExist

```


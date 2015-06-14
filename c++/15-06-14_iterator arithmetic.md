


For `vector<int> nums`. If nums.size() < k, then (nums.end() - k) is undefined.

So you can't compare nums.begin() with it.

Code:

  vector<int> nums = {1,2};
  cout << (nums.begin() < (nums.end() - 2)) << endl;  // on mac osx 10.10.2, clang-600.0.54, it returns 0


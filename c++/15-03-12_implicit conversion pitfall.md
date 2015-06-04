
# Pitfall 1

```cpp

vector<int> vec;
for (int i = 0; i < vec.size()-1; i++) {
  cout << vec[i] << endl;
}

```

For above code, vec.size() returns an unsigned integer. As a result,
vec.size()-1 > 0 . So there will be segmentation fault here.

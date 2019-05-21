# ReadableMap 转 Bundle

```
import android.os.Bundle
import com.facebook.react.bridge.ReadableArray
import com.facebook.react.bridge.ReadableMap
import com.facebook.react.bridge.ReadableType

object ParamsConventer {
    fun getBundleFromReadableMap(map: ReadableMap): Bundle {
        println("aaaa"+map)
        var result = Bundle()
        val iterator = map.keySetIterator()
        while (iterator.hasNextKey()) {
            val key = iterator.nextKey()
            val type = map.getType(key)
            when (type) {
                ReadableType.Null -> result.putString(key, null)
                ReadableType.Boolean -> result.putBoolean(key, map.getBoolean(key))
            // ReadableNativeArray 中把 number 转成了 double 处理
                ReadableType.Number -> result.putDouble(key, map.getDouble(key))
                ReadableType.String -> result.putString(key, map.getString(key))
                ReadableType.Map -> result.putBundle(key, getBundleFromReadableMap(map.getMap(key)))
                ReadableType.Array -> {
                    handleArray(result, key, map.getArray(key))
                }
                else -> result.putString(key, null)
            }
        }
        return result
    }

    private fun handleArray(result: Bundle, key: String, readableArray: ReadableArray?) {
        readableArray?.apply {
            if (this.size() < 1) {
                result.putStringArray(key, arrayOf<String>())
            } else {
                val arrayItemType = this.getType(0)
                when (arrayItemType) {
                    ReadableType.Null -> result.putStringArray(key, arrayOf<String>())
                    ReadableType.Boolean -> result.putBooleanArray(key, parseArray(this, Boolean::class.java).toBooleanArray())
                    ReadableType.Number -> result.putDoubleArray(key, parseArray(this, Double::class.java).toDoubleArray())
                    ReadableType.String -> result.putStringArray(key, parseArray(this, String::class.java).toTypedArray())
                    ReadableType.Map -> result.putParcelableArrayList(key, arrayListOf(* parseArray(this, Bundle::class.java)
                            .toTypedArray()))
                }
            }
        }
    }

    private fun <T> parseArray(readableArray: ReadableArray, clazz: Class<T>): List<T> {
        return readableArray.toArrayList().mapIndexed { index, any ->
            if (clazz == Bundle::class.java) {
                getBundleFromReadableMap(readableArray.getMap(index)) as T
            } else {
                any as T
            }
        }
    }
}
```


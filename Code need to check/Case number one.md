```kotlin
suspend fun Api.stickerPack(
    id: String,
    onError: ErrorBlock = null,
    onSuccess: SuccessBlock<StickerPack>
) = get("sticker-packs/${id}", onError = onError, onSuccess = onSuccess)


data class ApiArgs<T>(
    val onError: ErrorBlock = null,
    val onSuccess: SuccessBlock<T>
)

suspend fun Api.stickerPack(id: String, classarg args: ApiArgs<StickerPack>) = get("sticker-packs/${id}", args)
```
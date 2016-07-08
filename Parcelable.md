Parcelable


# implements Parcelable #
# public static final Parcelable.Creator<T> CREATOR #
    public static final Parcelable.Creator<MediaInfo> CREATOR = new Parcelable.Creator<MediaInfo>() {

        @Override
        public MediaInfo createFromParcel(Parcel source) {
            return new MediaInfo(source);
        }

        @Override
        public MediaInfo[] newArray(int size) {
            return new MediaInfo[size];
        }
    };

    public MediaInfo(Parcel source) {
        mDetailReady = (Boolean) source.readValue(null);
        collection = (MediaCollection) source.readValue(ClassLoader
                .getSystemClassLoader());
        uri = source.readString();
        modifiedTime = source.readString();
        createTime = source.readString();
        resolution = source.readString();
        orientation = source.readInt();
        takenDate = source.readString();
        size = source.readLong();
        mimeType = source.readString();
        duration = source.readInt();
        count = source.readInt();
        filename = source.readString();
        filepath = source.readString();
        screenSize = source.readLong();

        if ((Boolean) source.readValue(null)) {
            list = new ArrayList<MediaInfo>();
            int listSize = source.readInt();
            for (int i = 0; i < listSize; i++) {
                list.add((MediaInfo) source.readValue(ClassLoader
                        .getSystemClassLoader()));
            }
        }

        type = source.readInt();
        locked = (Boolean) source.readValue(null);
        frameRate = source.readInt();
        mHasSubList = (Boolean) source.readValue(null);
    }

# 重载方法 #

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeValue(mDetailReady);
        dest.writeValue(collection);
        dest.writeString(uri);
        dest.writeString(modifiedTime);
        dest.writeString(createTime);
        dest.writeString(resolution);
        dest.writeInt(orientation);
        dest.writeString(takenDate);
        dest.writeLong(size);
        dest.writeString(mimeType);
        dest.writeInt(duration);
        dest.writeInt(count);
        dest.writeString(filename);
        dest.writeString(filepath);
        dest.writeLong(screenSize);

        if (list != null) {
            dest.writeValue(true);
            dest.writeInt(list.size());
            for (int i = 0; i < list.size(); i++) {
                dest.writeValue(list.get(i));
            }
        } else {
            dest.writeValue(false);
        }

        dest.writeInt(type);
        dest.writeValue(locked);
        dest.writeInt(frameRate);
        dest.writeValue(mHasSubList);
    }

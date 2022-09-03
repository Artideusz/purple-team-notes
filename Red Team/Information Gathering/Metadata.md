# Metadata

Metadata can be found in many files such as:

- Images.
- Documents.
- Videos and Sound clips.

Metadata in such files can have crucial information, for example:

- Uncensored version of an image (in a form of a thumbnail).
- The device that the media was created on.
- The location of which the photo was taken.

*Bare in mind* that many services **do** clear metadata of an image sent before displaying it to someone, but some services do not do that. Facebook clears all metadata and replaces it with it's own. Gimp 2.10 by default preserves the device model used to take an image, but modifies other things, like the thumbnail content, by setting it the same as the cropped image, preventing information leaks.

One of the more useful tools to gather information from metadata is `exiftool` for Linux and Windows.
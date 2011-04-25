## Using jQuery-File-Upload with the Flask microframework
(or with Werkzeug, which is the base for Flask)

    # (c) 2011 by Thomas Waldmann
    # Licensed under MIT license.

    from flask import jsonify

    #           vvvvvvvv  this is the URL you need for action= in index.html of the demo app
    @app.route('/+upload', methods=['GET', 'POST'])
    def upload():
        if request.method == 'GET':
            # we are expected to return a list of dicts with infos about
            # the already available files:
            file_infos = []
            for file_name in list_files():
                file_url = url_for('show_file', file_name=file_name)
                file_size = get_file_size(file_name)
                file_infos.append(dict(name=file_name,
                                       size=file_size,
                                       url=file_url))
            return jsonify(files=file_infos)

        if request.method == 'POST':
            # we are expected to save the uploaded file and return some
            # infos about it:
            #                              vvvvvvvvv   this is the name for input type=file
            data_file = request.files.get('data_file')
            file_name = data_file.filename
            save_file(data_file, file_name)
            file_size = get_file_size(file_name)
            file_url = url_for('show_file', file_name=file_name)
            thumbnail_url = url_for('thumbnail', file_name=file_name)
            return jsonify(name=file_name,
                           size=file_size,
                           url=file_url,
                           thumbnail=thumbnail_url)

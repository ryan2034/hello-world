# hello-worldSkip to content
Learn Git and GitHub without any code!
Using the Hello World guide, you’ll start a branch, write comments, and open a pull request.


sphinx-doc
/
sphinx
Code
Issues
668
Pull requests
69
Actions
Projects
Wiki
Security
Insights
Respect "includehidden" for sub-toctrees as well

Previously, the includehidden method argument was only being taken into account
for top level toctrees. This meant that hidden subtree toctrees were still
being resolved if they exists below a non-hidden toctree.
 3.x  v3.4.3 
…
 1.1
@michaeljones
michaeljones committed on Jun 4, 2011
1 parent 6da34dd commit b4a6b2c3c130129e70a76e909a0f886df21d1f18
Showing  with 8 additions and 6 deletions.
 14  sphinx/environment.py 
@@ -1356,32 +1356,33 @@
                        # empty toc means: no titles will show up in the toctree	                        # empty toc means: no titles will show up in the toctree
                        self.warn(docname,	                        self.warn(docname,
                                  'toctree contains reference to document '	                                  'toctree contains reference to document '
                                  '%r that doesn\'t have a title: no link '	                                  '%r that doesn\'t have a title: no link '
                                  'will be generated' % ref, toctreenode.line)	                                  'will be generated' % ref, toctreenode.line)
                except KeyError:	                except KeyError:
                    # this is raised if the included file does not exist	                    # this is raised if the included file does not exist
                    self.warn(docname, 'toctree contains reference to '	                    self.warn(docname, 'toctree contains reference to '
                              'nonexisting document %r' % ref,	                              'nonexisting document %r' % ref,
                              toctreenode.line)	                              toctreenode.line)
                else:	                else:
                    # if titles_only is given, only keep the main title and	                    # if titles_only is given, only keep the main title and
                    # sub-toctrees	                    # sub-toctrees
                    if titles_only:	                    if titles_only:
                        # delete everything but the toplevel title(s)	                        # delete everything but the toplevel title(s)
                        # and toctrees	                        # and toctrees
                        for toplevel in toc:	                        for toplevel in toc:
                            # nodes with length 1 don't have any children anyway	                            # nodes with length 1 don't have any children anyway
                            if len(toplevel) > 1:	                            if len(toplevel) > 1:
                                subtrees = toplevel.traverse(addnodes.toctree)	                                subtrees = toplevel.traverse(addnodes.toctree)
                                toplevel[1][:] = subtrees	                                toplevel[1][:] = subtrees
                    # resolve all sub-toctrees	                    # resolve all sub-toctrees
                    for toctreenode in toc.traverse(addnodes.toctree):	                    for toctreenode in toc.traverse(addnodes.toctree):
                        i = toctreenode.parent.index(toctreenode) + 1	                        if not ( toctreenode.get('hidden', False) and not includehidden ):
                        for item in _entries_from_toctree(toctreenode,	                            i = toctreenode.parent.index(toctreenode) + 1
                                                          subtree=True):	                            for item in _entries_from_toctree(toctreenode,
                            toctreenode.parent.insert(i, item)	                                                              subtree=True):
                            i += 1	                                toctreenode.parent.insert(i, item)
                        toctreenode.parent.remove(toctreenode)	                                i += 1
                            toctreenode.parent.remove(toctreenode)
                    if separate:	                    if separate:
                        entries.append(toc)	                        entries.append(toc)
                    else:	                    else:
@@ -1740,3 +1741,4 @@ def check_consistency(self):
                if 'orphan' in self.metadata[docname]:	                if 'orphan' in self.metadata[docname]:
                    continue	                    continue
                self.warn(docname, 'document isn\'t included in any toctree')	                self.warn(docname, 'document isn\'t included in any toctree')

0 comments on commit b4a6b2c

Leave a comment
 You’re not receiving notifications from this thread.
© 2021 GitHub, Inc.
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About

